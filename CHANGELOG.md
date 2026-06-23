# Changelog

All notable changes to this project will be documented in this file.

## [1.3.0] - 2026-06-23

### Changed - UI/UX Improvements for General Users

**Simplified Interface:**
- ❌ Removed technical export buttons (JSON, GeoJSON) from main view
- 🎯 Cleaner, more focused user interface for non-technical users

**User-Friendly Language:**
- "Fit ke UGM" → "Tampilkan Semua"
- "Reset filter" → "Reset"
- "Tampil di map" → "Ditampilkan"
- "Berkoordinat" → "Lokasi Valid"
- "Buka Maps" → "Buka di Google Maps"
- "Website" → "Kunjungi Website"

**Better Guidance:**
- Search placeholder: "Ketik nama fakultas, gedung, atau layanan..." (more descriptive)
- Checkbox: "Tampilkan lokasi di luar kampus utama" (clearer scope)
- Footer note: Comprehensive explanation about data source and usage

**Simplified Detail View:**
- ❌ Removed technical fields: ID, Baris Excel, separate Lat/Long
- ✅ Consolidated coordinates into single field
- ❌ Removed OpenStreetMap button (not relevant for general users)
- ✅ Kept only essential info: Koordinat, Lokasi, Google Maps link, Website

**Professional Messaging:**
- Brand tagline: More welcoming and descriptive
- Empty state: Helpful suggestion to try different keywords
- Error messages: Friendly and actionable
- Loading state: Clear progress indication

---

## [1.2.0] - 2026-06-23

### Added - Performance & Security
- Debounce search input (200ms) for better performance
- O(1) facility lookup using Map data structure
- Selective DOM updates (class toggle vs full rebuild)

### Fixed - Security
- XSS prevention with custom esc() helper
- All external links with rel="noopener noreferrer"

### Changed
- Switched to CARTO basemap tiles (more reliable than OSM direct)
- Added Escape key to close detail drawer
- Added aria-label for accessibility

---

## [1.1.0] - 2026-06-23

### Added - Documentation
- Complete stack documentation (STACK.md)
- Architecture diagrams (ARCHITECTURE.md)
- Developer quick reference (QUICKREF.md)
- GitHub deployment guide (PUSH_TO_GITHUB.md)
- AI agent guidelines (.github/copilot-instructions.md)

---

## [1.0.0] - 2026-06-23

### Initial Release
- Zero-build static web app with Leaflet.js 1.9.4
- 98 UGM facilities with interactive map
- Search and filter functionality
- Marker clustering for performance
- Category-based color coding (30 categories)
- Responsive design (mobile-friendly)
- GeoJSON export support
- Campus scope filtering (main campus vs extended)

**Tech Stack:**
- Leaflet.js 1.9.4 (mapping)
- Leaflet.markercluster 1.5.3 (clustering)
- CARTO basemaps (tile provider)
- Vanilla ES6+ JavaScript (no framework)
- Inline CSS3 (no preprocessor)

---

## Future Considerations

### Possible Features
- [ ] Image thumbnails for facilities
- [ ] Directions from current location
- [ ] Share facility link (deep linking)
- [ ] Print-friendly facility list
- [ ] Multi-language support (EN/ID)
- [ ] Dark mode toggle
- [ ] Facility reviews/comments (requires backend)
- [ ] Opening hours information
- [ ] 360° virtual tours integration
- [ ] Accessibility improvements (screen reader optimization)

### Technical Improvements
- [ ] Service Worker for offline support
- [ ] IndexedDB for client-side caching
- [ ] Progressive Web App (PWA) manifest
- [ ] Lazy loading for facility images
- [ ] Advanced search (fuzzy matching)
- [ ] Custom marker icons per category
- [ ] Route planning between facilities

---

**Versioning:** This project follows [Semantic Versioning](https://semver.org/).
- MAJOR version for incompatible API/data format changes
- MINOR version for new features in a backwards compatible manner
- PATCH version for backwards compatible bug fixes
