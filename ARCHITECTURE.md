# Architecture Diagram

```
┌─────────────────────────────────────────────────────────────────┐
│                    UGM FACILITY MAPS                            │
│                  (Static Web Application)                       │
└─────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────┐
│  CLIENT BROWSER                                                 │
│  ┌───────────────────────────────────────────────────────────┐  │
│  │  index.html (Single File ~15KB)                           │  │
│  │  ┌─────────────┐  ┌─────────────┐  ┌──────────────────┐  │  │
│  │  │   <style>   │  │   <body>    │  │    <script>      │  │  │
│  │  │ Inline CSS  │  │  HTML DOM   │  │  Vanilla ES6 JS  │  │  │
│  │  │   ~3 KB     │  │  Structure  │  │     ~8 KB        │  │  │
│  │  └─────────────┘  └─────────────┘  └──────────────────┘  │  │
│  └───────────────────────────────────────────────────────────┘  │
│                              │                                   │
│                              ├─ CDN Dependencies                 │
│                              │                                   │
│  ┌────────────────┐  ┌──────────────────┐  ┌─────────────────┐ │
│  │  Leaflet.js    │  │  Markercluster   │  │  CARTO Tiles    │ │
│  │   ~145 KB      │  │     ~55 KB       │  │   (Dynamic)     │ │
│  │  unpkg.com     │  │   unpkg.com      │  │  basemaps.      │ │
│  │                │  │                  │  │  cartocdn.com   │ │
│  └────────────────┘  └──────────────────┘  └─────────────────┘ │
│                              │                                   │
│                              ├─ Data Files (fetch)               │
│                              │                                   │
│  ┌────────────────────────┐  ┌───────────────────────────────┐ │
│  │  facilities.json       │  │  facilities.geojson           │ │
│  │  (Runtime Data)        │  │  (Download Only)              │ │
│  │  ~120 KB               │  │  ~180 KB                      │ │
│  │  ├─ metadata           │  │  ├─ FeatureCollection         │ │
│  │  └─ facilities[98]     │  │  └─ features[98]              │ │
│  └────────────────────────┘  └───────────────────────────────┘ │
└─────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────┐
│  STATE MANAGEMENT (In-Memory)                                   │
│  ┌────────────────┐  ┌─────────────────┐  ┌─────────────────┐  │
│  │ allFacilities  │  │  facilityById   │  │   markerById    │  │
│  │   Array[98]    │  │    Map (O1)     │  │   Map (O1)      │  │
│  └────────────────┘  └─────────────────┘  └─────────────────┘  │
│  ┌────────────────┐  ┌─────────────────┐  ┌─────────────────┐  │
│  │   filtered     │  │    cluster      │  │    activeId     │  │
│  │   Array[n]     │  │  MarkerCluster  │  │   String|null   │  │
│  └────────────────┘  └─────────────────┘  └─────────────────┘  │
└─────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────┐
│  UI COMPONENTS                                                   │
│  ┌──────────────────────┐       ┌──────────────────────────┐   │
│  │  SIDEBAR (aside)     │       │  MAP CONTAINER (main)    │   │
│  │  ┌────────────────┐  │       │  ┌────────────────────┐  │   │
│  │  │ Brand Header   │  │       │  │  Leaflet Map       │  │   │
│  │  │ Statistics     │  │       │  │  (Markers/Cluster) │  │   │
│  │  │ Search Input   │  │       │  └────────────────────┘  │   │
│  │  │ Category Filter│  │       │  ┌────────────────────┐  │   │
│  │  │ Checkbox       │  │       │  │  Top Info Pill     │  │   │
│  │  │ Action Buttons │  │       │  └────────────────────┘  │   │
│  │  │ Facility List  │  │       │  ┌────────────────────┐  │   │
│  │  │  (Scrollable)  │  │       │  │  Legend (Bottom)   │  │   │
│  │  └────────────────┘  │       │  └────────────────────┘  │   │
│  └──────────────────────┘       │  ┌────────────────────┐  │   │
│         410px fixed              │  │ Detail Drawer      │  │   │
│                                  │  │ (Overlay/Hidden)   │  │   │
│                                  │  └────────────────────┘  │   │
│                                  └──────────────────────────┘   │
│                                      Responsive (flex 1)        │
└─────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────┐
│  DATA FLOW                                                       │
│                                                                  │
│  1. Load index.html                                             │
│      ↓                                                           │
│  2. Load CSS from CDN (Leaflet + Markercluster)                │
│      ↓                                                           │
│  3. Load JS from CDN (Leaflet + Markercluster)                 │
│      ↓                                                           │
│  4. Execute inline <script>                                     │
│      ↓                                                           │
│  5. setupMap() → Initialize Leaflet + CARTO tiles              │
│      ↓                                                           │
│  6. loadData() → fetch('facilities.json')                      │
│      ↓                                                           │
│  7. Build facilityById Map (O(n))                              │
│      ↓                                                           │
│  8. applyFilters() → filter by campus_scope                    │
│      ↓                                                           │
│  9. renderAll() → renderMarkers() + renderList()               │
│      ↓                                                           │
│ 10. fitToData() → Auto-zoom to facility bounds                 │
│      ↓                                                           │
│ 11. User Interaction:                                           │
│     - Search input → debounce(applyFilters, 200)               │
│     - Category change → applyFilters() immediate                │
│     - Checkbox toggle → applyFilters() immediate                │
│     - Card click → showDetail(id, pan=true)                    │
│     - Marker click → showDetail(id, pan=true)                  │
│     - Escape key → close drawer                                 │
│     - Reset button → clear filters + fitToData()               │
└─────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────┐
│  DEPLOYMENT OPTIONS                                              │
│                                                                  │
│  Option A: GitHub Pages (Recommended)                           │
│  ┌────────────────────────────────────────────────────────────┐ │
│  │  1. Push to GitHub repo                                    │ │
│  │  2. Enable Pages in Settings                               │ │
│  │  3. Deploy from main branch                                │ │
│  │  Result: https://username.github.io/ugm_maps_deploy/       │ │
│  └────────────────────────────────────────────────────────────┘ │
│                                                                  │
│  Option B: Netlify / Vercel / Cloudflare Pages                  │
│  ┌────────────────────────────────────────────────────────────┐ │
│  │  1. Drag & drop 3 files                                    │ │
│  │  2. Auto-deploy with HTTPS                                 │ │
│  │  Result: https://ugm-maps.netlify.app                      │ │
│  └────────────────────────────────────────────────────────────┘ │
│                                                                  │
│  Option C: Traditional Web Hosting                              │
│  ┌────────────────────────────────────────────────────────────┐ │
│  │  1. FTP upload: index.html + 2 JSON files                  │ │
│  │  2. Point domain to folder                                 │ │
│  │  Result: https://maps.ugm.ac.id                            │ │
│  └────────────────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────┐
│  PERFORMANCE METRICS                                             │
│                                                                  │
│  Initial Load:                                                   │
│  ├─ HTML/CSS/JS (cached):        ~15 KB (< 50ms)               │
│  ├─ Leaflet.js (CDN cached):     ~145 KB (< 200ms)             │
│  ├─ Markercluster (CDN cached):  ~55 KB (< 100ms)              │
│  ├─ facilities.json:              ~120 KB (< 150ms)             │
│  ├─ CARTO tiles (lazy):           ~500 KB (< 300ms)             │
│  └─ Total FCP:                    ~800ms (Good)                 │
│                                                                  │
│  Search Performance:                                             │
│  ├─ Without debounce:             10-15ms per keystroke         │
│  └─ With debounce (200ms):        1 call per typing session    │
│                                                                  │
│  Rendering Performance:                                          │
│  ├─ renderMarkers():              ~30ms (98 markers)            │
│  ├─ renderList():                 ~20ms (98 cards)              │
│  ├─ showDetail() (old):           ~50ms (full list rebuild)    │
│  └─ showDetail() (optimized):     ~5ms (class toggle only)     │
│                                                                  │
│  Memory Usage:                                                   │
│  ├─ JavaScript Heap:              ~8 MB                         │
│  ├─ DOM Nodes:                    ~300 elements                 │
│  └─ Leaflet Markers:              98 instances + clustering     │
└─────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────┐
│  SECURITY MODEL                                                  │
│                                                                  │
│  XSS Prevention:                                                 │
│  ├─ Custom esc() function for all user data                    │
│  ├─ No innerHTML with untrusted content                        │
│  └─ Template literals with escaped variables                   │
│                                                                  │
│  Link Security:                                                  │
│  ├─ All target="_blank" with rel="noopener noreferrer"        │
│  └─ Prevents reverse tabnapping attacks                        │
│                                                                  │
│  Data Integrity:                                                 │
│  ├─ Static JSON (no user-generated content)                    │
│  ├─ No database (no SQL injection risk)                        │
│  └─ No server-side processing (no RCE risk)                    │
│                                                                  │
│  Privacy:                                                        │
│  ├─ No analytics / tracking by default                         │
│  ├─ No cookies / localStorage usage                            │
│  └─ No user authentication required                            │
└─────────────────────────────────────────────────────────────────┘
```
