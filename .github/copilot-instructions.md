# GitHub Copilot Instructions – UGM Facility Maps

## Project Overview
A **zero-build, static web app** — one HTML file + two JSON data files, no bundler, no npm. All JavaScript is inline in `index.html`. Served locally via Laragon; deployable to any static host (GitHub Pages, etc.).

**Tech Stack:** Leaflet.js 1.9.4 + Leaflet.markercluster 1.5.3 + CARTO basemaps + Vanilla ES6+ JavaScript  
**Full Documentation:** See [STACK.md](../STACK.md) for complete technology stack details.

## Architecture
```
index.html          ← entire app: CSS, HTML layout, and all JS logic (inline)
facilities.json     ← primary runtime data source (fetched via fetch())
facilities.geojson  ← export/download format only; NOT loaded by the app at runtime
summary.md          ← human-readable table of all facilities (reference only)
README.md           ← deploy instructions
```

The app fetches `facilities.json` at startup (`loadData()`). `facilities.geojson` is only linked as a download button in the sidebar.

## Facility Object Schema
Every item in `facilities.facilities[]` has:
```json
{
  "id": "UGM-FAC-001",          // sequential, zero-padded 3-digit
  "name": "...",
  "category": "Fakultas",       // must match a key in CATEGORY_COLORS
  "description": "...",
  "latitude": -7.7676797,       // null/absent = excluded from map
  "longitude": 110.3785754,
  "maps_url": "https://...",    // nullable
  "website_url": "https://...", // nullable
  "image_url": null,
  "source_row": 2,              // row number in origin Excel file
  "coordinate_source": "extracted_from_uploaded_google_maps_link",
  "slug": "direktorat-keuangan-ugm",
  "campus_scope": "kampus_utama",             // or "ekstensi_luar_kampus_utama"
  "campus_scope_label": "Kampus Utama UGM"
}
```

## Critical Patterns

### Category Colors
All categories must have an entry in the `CATEGORY_COLORS` object at the top of the `<script>` block in `index.html`. The same map must be updated in **both** `index.html` (JS constant) and the data files when adding a new category. Missing entries fall back to `FALLBACK_COLOR = '#334155'`.

### Coordinate Validity
`validCoord(f)` — checks `Number.isFinite(f.latitude) && Number.isFinite(f.longitude)`. Facilities without valid coordinates are silently excluded from the map and filter counts; they still appear in `allFacilities`.

### Filtering Pipeline
`applyFilters()` → rebuilds `filtered[]` from `allFacilities[]` using:
1. `validCoord(f)` guard
2. Free-text search across `name + category + description`
3. Category dropdown
4. `includeExtended` checkbox — hides `campus_scope === 'ekstensi_luar_kampus_utama'` by default

### XSS Escaping
Use the local `esc(v)` helper (defined in JS) for all user-visible data injected into HTML strings. Do **not** add a library for this — the helper covers the required characters.

### Debounce
The `debounce(fn, ms)` helper is defined just above the event listeners. Apply it to any input that triggers `applyFilters()` (currently `searchBox` with 200 ms). Do **not** debounce `select` / `checkbox` change events.

### Map Interaction
- Markers are grouped via `L.markerClusterGroup` (stored as `cluster`).
- `markerById` (Map) links facility `id` → Leaflet marker for programmatic pan/popup.
- `facilityById` (Map) links facility `id` → facility object for O(1) lookup (populated in `loadData`).
- `showDetail(id, pan=true)` is attached to `window` because it is called from popup HTML strings.
- `showDetail` updates the active card by toggling `.active` class directly on the DOM element — **do not call `renderList()` inside `showDetail`** as it would trigger a full DOM rebuild just for a class change.

## Security & Accessibility Conventions
- All `target="_blank"` links must include `rel="noopener noreferrer"` to prevent reverse tabnapping.
- Use `aria-label` on icon-only interactive elements (e.g., the drawer close button).
- The `Escape` key closes the detail drawer via a `document keydown` listener.

## Local Development
Must be served over HTTP — `fetch('facilities.json')` fails on `file://`. Use Laragon:
```
http://localhost/ugm_maps_deploy/
```
No install step, no build step, no watch mode needed.

## Updating Facility Data
Data originates from `maps-ugm.xlsx`. To update:
1. Re-extract/re-generate `facilities.json` and `facilities.geojson` from the Excel source.
2. Keep `metadata.total_records`, `metadata.categories[]`, `metadata.main_campus_records`, and `metadata.extended_records` in sync.
3. If new categories are introduced, add them to `CATEGORY_COLORS` in `index.html`.
4. ID format must remain `UGM-FAC-NNN` (zero-padded, sequential).

## Deployment
Copy three files to any static host:
```
index.html  facilities.json  facilities.geojson
```
No server-side processing required.

## Language & Locale
- UI labels and data are in **Bahasa Indonesia**.
- Default map center: `[-7.7708604, 110.3778693]`, zoom 15 (UGM main campus, Yogyakarta).
- Main campus bounding box: south `-7.786`, west `110.36`, north `-7.758`, east `110.39`.
