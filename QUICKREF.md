# Quick Reference Card

## 🚀 Quick Start

```bash
# Clone or download
cd ugm_maps_deploy

# Start local server (pilih salah satu)
python -m http.server 8000
php -S localhost:8000
npx serve .

# Atau gunakan Laragon
# → http://localhost/ugm_maps_deploy/
```

---

## 📁 File Structure

```
ugm_maps_deploy/
├── index.html              # Main app (240 lines, single file)
├── facilities.json         # Runtime data (120 KB, 98 facilities)
├── facilities.geojson      # Export format (180 KB)
├── README.md               # Getting started guide
├── STACK.md                # Technology documentation (full)
├── ARCHITECTURE.md         # Visual diagrams
├── summary.md              # Human-readable facility table
└── .github/
    └── copilot-instructions.md  # AI agent guidelines
```

---

## 🔧 Key Functions

| Function | Purpose | Complexity |
|---|---|---|
| `loadData()` | Fetch JSON, build Maps | O(n) |
| `applyFilters()` | Filter facilities | O(n) |
| `renderMarkers()` | Update map pins | O(n) |
| `renderList()` | Update sidebar cards | O(n) |
| `showDetail(id)` | Show facility detail | O(1) |
| `fitToData()` | Auto-zoom map | O(n) |
| `debounce(fn, ms)` | Rate-limit function | O(1) |

---

## 🎨 Key Variables

```javascript
// State
allFacilities  // Array[98]  - All data from JSON
filtered       // Array[n]   - After filters applied
activeId       // String     - Currently selected facility ID

// Lookup Tables (O(1) access)
facilityById   // Map<id, facility>  - Fast facility lookup
markerById     // Map<id, marker>    - Fast marker lookup

// Leaflet Objects
map            // L.Map      - Main map instance
cluster        // L.MarkerClusterGroup - Clustering layer
```

---

## 🎯 Common Tasks

### Add New Facility
1. Edit `facilities.json` → add to `facilities[]` array
2. Increment `metadata.total_records`
3. Update `metadata.categories[]` if new category
4. Add color to `CATEGORY_COLORS` in `index.html` if needed
5. Test with `http://localhost/ugm_maps_deploy/`

### Change Map Style
```javascript
// In setupMap() function (line ~131)
L.tileLayer('https://{s}.basemaps.cartocdn.com/light_all/{z}/{x}/{y}{r}.png', {
  // Options:
  // light_all   → Light theme (current)
  // dark_all    → Dark theme
  // rastertiles/voyager → Voyager theme
```

### Customize Colors
```javascript
// Line ~120 in index.html
const CATEGORY_COLORS = {
  "Fakultas": "#0891b2",     // Teal
  "Kantin": "#0f766e",       // Darker teal
  // Add more as needed
};
```

### Adjust Search Debounce
```javascript
// Line ~225 in index.html
$('searchBox').addEventListener('input', debounce(applyFilters, 200));
//                                                                ↑
//                                                    Change this value (ms)
```

---

## 🐛 Debugging

### Check Browser Console
```javascript
// Available in console:
allFacilities      // See all data
filtered           // See current filtered results
facilityById       // Lookup facility by ID
map                // Access Leaflet map object
cluster            // Access marker cluster
```

### Common Errors

| Error | Cause | Fix |
|---|---|---|
| `Failed to fetch` | Using `file://` protocol | Use HTTP server |
| `Cannot read property 'latitude'` | Invalid coordinate data | Check `validCoord(f)` |
| Tiles not loading | CARTO CDN issue | Check network tab |
| Map not centered | Invalid center coords | Check `setupMap()` |

---

## 🔍 Search for Patterns

```bash
# Find all event listeners
grep "addEventListener" index.html

# Find all DOM manipulations
grep "getElementById\|querySelector" index.html

# Find all external links
grep "target=\"_blank\"" index.html

# Find all data access
grep "facilityById\|markerById" index.html
```

---

## 📊 Performance Tips

| Tip | Impact |
|---|---|
| Keep `filtered[]` small | Faster rendering |
| Use debounce on input | Fewer filter calls |
| Use O(1) Map lookups | Instant access |
| Avoid `renderList()` in loops | Prevents reflow |
| Lazy load images (future) | Faster initial load |

---

## 🔒 Security Checklist

- ✅ All user data passed through `esc()` function
- ✅ All `target="_blank"` have `rel="noopener noreferrer"`
- ✅ No `eval()` or `Function()` constructor
- ✅ No inline event handlers (all via `addEventListener`)
- ✅ HTTPS for all CDN resources
- ✅ No cookies or localStorage (stateless)

---

## 📦 Deployment Checklist

- [ ] Test on `http://localhost` first
- [ ] Verify all 98 facilities load
- [ ] Test search functionality
- [ ] Test category filter
- [ ] Test "extended campus" checkbox
- [ ] Check mobile responsive design
- [ ] Verify map tiles load correctly
- [ ] Test detail drawer (click cards/markers)
- [ ] Verify JSON/GeoJSON download buttons
- [ ] Test keyboard navigation (Escape key)
- [ ] Check browser console for errors
- [ ] Test on Chrome, Firefox, Safari, Edge

---

## 🌐 CDN Resources

| Resource | URL | Fallback |
|---|---|---|
| **Leaflet CSS** | unpkg.com/leaflet@1.9.4 | Download & self-host |
| **Leaflet JS** | unpkg.com/leaflet@1.9.4 | Download & self-host |
| **Markercluster** | unpkg.com/leaflet.markercluster@1.5.3 | Download & self-host |
| **CARTO Tiles** | basemaps.cartocdn.com | Switch to OSM direct |

### Self-Host CDN (Optional)
```bash
# Download dependencies
curl -O https://unpkg.com/leaflet@1.9.4/dist/leaflet.js
curl -O https://unpkg.com/leaflet@1.9.4/dist/leaflet.css
curl -O https://unpkg.com/leaflet.markercluster@1.5.3/dist/leaflet.markercluster.js
curl -O https://unpkg.com/leaflet.markercluster@1.5.3/dist/MarkerCluster.css

# Update index.html paths
<link href="./leaflet.css" />
<script src="./leaflet.js"></script>
```

---

## 📞 Support

| Question | Resource |
|---|---|
| Architecture overview | [ARCHITECTURE.md](ARCHITECTURE.md) |
| Technology stack | [STACK.md](STACK.md) |
| Getting started | [README.md](README.md) |
| AI agent guidelines | [.github/copilot-instructions.md](.github/copilot-instructions.md) |
| Leaflet docs | https://leafletjs.com/reference.html |
| Markercluster docs | https://github.com/Leaflet/Leaflet.markercluster |

---

**Last Updated:** June 23, 2026  
**Maintainer:** UGM IT Team  
**License:** (To be determined)
