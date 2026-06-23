# Evaluasi Teknologi Maps - Analisis Perbandingan

## 1. Status Quo: Leaflet.js + CARTO

### Kelebihan Saat Ini
- ✅ Sangat ringan (~145 KB)
- ✅ Zero-build setup
- ✅ Cukup untuk kasus penggunaan dasar
- ✅ Loading cepat

### Kekurangan Saat Ini
- ❌ UI terbatas (minimal popups, no street view)
- ❌ Tidak ada info jalan/nama jalan yang interaktif
- ❌ Tidak ada traffic/transit information
- ❌ Popup basik tanpa styling advanced
- ❌ Tidak ada clustering advanced dengan drill-down
- ❌ Limited basemap styles
- ❌ Tidak bisa search by address

---

## 2. Alternatif Open-Source & Free

### A. Maplibre GL JS (⭐ Recommended)
```html
<link href='https://unpkg.com/maplibre-gl@latest/dist/maplibre-gl.css' rel='stylesheet' />
<script src='https://unpkg.com/maplibre-gl@latest/dist/maplibre-gl.js'></script>
```

**Kelebihan:**
- ✅ WebGL rendering (lebih smooth untuk banyak marker)
- ✅ Vector tile support (tampilan lebih modern)
- ✅ Style layer yang sophisticated
- ✅ 3D building visualization
- ✅ Advanced styling (Mapbox GL style format)
- ✅ Better performance untuk 1000+ markers
- ✅ Open-source (BSD license)
- ✅ Support berbagai tile provider

**Kekurangan:**
- ❌ File lebih besar (~500 KB)
- ❌ Kurva belajar lebih steep

**Bundle Size:** ~500 KB (vs Leaflet 145 KB)
**Use Case:** Aplikasi maps profesional dengan banyak data

---

### B. OpenLayers
```html
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/ol@v7.5.2/ol.css">
<script src="https://cdn.jsdelivr.net/npm/ol@v7.5.2/dist/ol.js"></script>
```

**Kelebihan:**
- ✅ Very powerful & feature-rich
- ✅ Excellent for GIS applications
- ✅ Support multiple data formats (GeoJSON, WMS, etc)
- ✅ Advanced interaction tools
- ✅ Great for professional mapping

**Kekurangan:**
- ❌ Sangat besar (~1+ MB uncompressed)
- ❌ Kompleks untuk case sederhana
- ❌ Dokumentasi kurang user-friendly
- ❌ Overkill untuk aplikasi ini

**Bundle Size:** ~1 MB+
**Use Case:** GIS applications profesional

---

### C. Deck.gl (by Uber)
```html
<script src="https://cdn.jsdelivr.net/npm/deck.gl@^8.9.0/dist.min.js"></script>
```

**Kelebihan:**
- ✅ Amazing visualizations (heat maps, 3D)
- ✅ WebGL-based (very performant)
- ✅ Great for large datasets
- ✅ Modern, responsive UI
- ✅ Open-source (MIT)

**Kekurangan:**
- ❌ Fokus pada visualization, bukan traditional mapping
- ❌ Kurva belajar tinggi
- ❌ Butuh pengetahuan D3.js/WebGL
- ❌ Tidak ideal untuk tile-based maps tradisional

**Bundle Size:** ~2+ MB
**Use Case:** Advanced data visualization & analytics

---

### D. Folium (Python - Backend)
```python
import folium
m = folium.Map(location=[lat, lon], zoom_start=15)
folium.Marker([lat, lon], popup='Facility').add_to(m)
m.save('index.html')
```

**Kelebihan:**
- ✅ Easy to use
- ✅ Built on Leaflet (familiar)
- ✅ Python-based (simpler for some)
- ✅ Quick prototyping

**Kekurangan:**
- ❌ Requires build step
- ❌ Tidak cocok untuk zero-build approach
- ❌ Less flexibility than vanilla JS
- ❌ Requires Python/server

---

### E. Kepler.gl
```html
<script src="https://uber.github.io/kepler.gl/demo/keplergl.js"></script>
```

**Kelebihan:**
- ✅ Beautiful 3D visualizations
- ✅ Data analytics ready
- ✅ Geospatial analysis tools
- ✅ Export to various formats

**Kekurangan:**
- ❌ Overkill untuk use case ini
- ❌ Lebih untuk data scientists
- ❌ Kompleks untuk simple facility map

---

## 3. Tile Provider Alternatif (untuk Visual Enhancement)

### Current: CARTO Light
```
https://{s}.basemaps.cartocdn.com/light_all/{z}/{x}/{y}{r}.png
```

### Alternatif Tersedia:

#### A. Stamen Maps (via OpenStreetMap)
```
https://stamen-tiles.a.ssl.fastly.net/terrain/{z}/{x}/{y}.png
https://stamen-tiles.a.ssl.fastly.net/toner/{z}/{x}/{y}.png
```
**Keuntungan:** Lebih detail, artistic styling

#### B. OpenStreetMap (Direct)
```
https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png
```
**Keuntungan:** Paling detail, community-driven

#### C. Esri World Imagery
```
https://server.arcgisonline.com/ArcGIS/rest/services/World_Imagery/MapServer/tile/{z}/{y}/{x}
```
**Keuntungan:** Satellite/imagery, detailed

#### D. Jawg Maps
```
https://{s}.tile.jawg.io/jawg-dark/{z}/{x}/{y}{r}.png
```
**Keuntungan:** Modern style, multiple themes

---

## 4. Feature Enhancements Possible

### Dengan Maplibre GL JS:
- ✅ **3D Buildings** - Visualisasi gedung 3D
- ✅ **Heatmaps** - Konsentrasi fasilitas
- ✅ **Custom Styling** - Design sesuai brand UGM
- ✅ **Street View Integration** - Google Street View
- ✅ **Search/Geocoding** - Cari by address
- ✅ **Directions** - Petunjuk rute (OpenRouteService)
- ✅ **Advanced Clustering** - Lebih informatif
- ✅ **Light/Dark Mode** - User preference

### Dengan Leaflet.js (Plugins):
- ✅ `leaflet-control-geocoder` - Geocoding/search
- ✅ `leaflet-routing-machine` - Routing/directions
- ✅ `leaflet-heat` - Heatmaps
- ✅ `leaflet-draw` - Drawing tools
- ✅ `leaflet.fullscreen` - Fullscreen mode

---

## 5. Rekomendasi untuk UGM Maps

### Option 1: Upgrade Leaflet ke Maplibre GL JS (🥇 Best Balance)

**Alasan:**
- Modern WebGL rendering
- Lebih smooth untuk future expansion
- Tidak terlalu kompleks untuk digunakan
- Bundle size reasonable (~500 KB)
- Excellent documentation
- Large community support
- MIT License (open-source)

**Implementation Cost:** Medium
**Learning Curve:** Medium
**Performance Gain:** High
**Maintenance:** Easy

**Potential Features:**
- 3D buildings visualization
- Advanced heatmaps
- Custom style layers
- Better zoom animations
- Smooth vector rendering

---

### Option 2: Keep Leaflet + Add Plugins (✅ Quick Enhancement)

**Plugins to Add:**
```html
<!-- Geocoder/Search -->
<link rel="stylesheet" href="https://unpkg.com/leaflet-control-geocoder/dist/Control.Geocoder.css" />
<script src="https://unpkg.com/leaflet-control-geocoder/dist/Control.Geocoder.js"></script>

<!-- Routing/Directions -->
<link rel="stylesheet" href="https://unpkg.com/leaflet-routing-machine@3.2.12/dist/leaflet-routing-machine.css" />
<script src="https://unpkg.com/leaflet-routing-machine@3.2.12/dist/leaflet-routing-machine.js"></script>

<!-- Heatmap -->
<script src="https://unpkg.com/leaflet.heat/dist/leaflet-heat.js"></script>

<!-- Fullscreen -->
<link rel="stylesheet" href="https://unpkg.com/leaflet-fullscreen/dist/leaflet.fullscreen.css"/>
<script src="https://unpkg.com/leaflet-fullscreen/dist/Leaflet.fullscreen.js"></script>
```

**Kelebihan:**
- ✅ Minimal changes to existing code
- ✅ Each plugin independent
- ✅ Progressive enhancement
- ✅ Keep bundle size manageable

**Implementation Cost:** Low
**Learning Curve:** Low
**Performance Gain:** Medium
**Maintenance:** Easy

---

### Option 3: Hybrid Approach (🏆 Recommended)

**Keep Leaflet BUT add:**
1. ✅ **Search/Geocoding** → leaflet-control-geocoder
2. ✅ **Better clustering** → Upgrade markercluster config
3. ✅ **Heatmap layer** → leaflet.heat (optional toggle)
4. ✅ **Better tile provider** → Switch to Stamen or Esri
5. ✅ **Directions** → leaflet-routing-machine (optional)
6. ✅ **Fullscreen** → leaflet-fullscreen

**Tidak perlu migrate semuanya sekaligus, tapi bisa di-enhance gradually.**

---

## 6. Comparison Matrix

| Feature | Leaflet | Leaflet + Plugins | Maplibre GL | OpenLayers | Deck.gl |
|---|---|---|---|---|---|
| **Bundle Size** | 145 KB | 300-500 KB | 500 KB | 1+ MB | 2+ MB |
| **Setup Complexity** | Very Easy | Easy | Medium | Complex | Complex |
| **Performance** | Good | Good | Excellent | Good | Excellent |
| **3D Support** | No | Limited | Yes | Limited | Yes |
| **Heatmaps** | Via Plugin | Yes | Yes | Yes | Yes |
| **Search** | Via Plugin | Yes | Via Plugin | Yes | No |
| **Routing** | Via Plugin | Yes | Via Plugin | Yes | No |
| **Learning Curve** | Shallow | Shallow | Medium | Steep | Steep |
| **Community** | Large | Large | Growing | Large | Growing |
| **License** | BSD | Mixed | OSS | OSS | MIT |
| **Ideal For** | Simple maps | Enhanced maps | Professional | GIS/Analytics | Data Viz |

---

## 7. Implementation Roadmap

### Phase 1 (Quick Win - 1-2 hari)
```
Leaflet + Plugins Enhancement
├─ Add geocoder/search
├─ Upgrade tile provider (Stamen/Esri)
├─ Add fullscreen toggle
└─ Improve clustering visualization
```

### Phase 2 (Medium Enhancement - 1-2 minggu)
```
Additional Features
├─ Add routing/directions (optional)
├─ Heatmap layer toggle
├─ Category-based heatmaps
└─ Advanced filtering
```

### Phase 3 (Full Migration - 2-4 minggu, optional)
```
Migrate to Maplibre GL JS
├─ Rewrite core mapping
├─ Implement 3D buildings
├─ Custom style layers
├─ Better animations
└─ Performance optimization
```

---

## 8. Recommended Stack untuk UGM

### **Short Term (Next Sprint)**
```
Leaflet.js 1.9.4
+ leaflet-control-geocoder (search)
+ leaflet.heat (optional heatmap)
+ leaflet-fullscreen (fullscreen mode)
+ Stamen/Esri tiles (better visuals)
```

**Why:**
- Minimal disruption to existing code
- Each enhancement is optional (progressive)
- Keep bundle size reasonable
- User immediately sees improvements
- Low risk, high reward

---

### **Long Term (Future Release)**
```
Consider migration to:
├─ Maplibre GL JS (if need 3D/advanced styling)
└─ Or keep Leaflet if current experience is sufficient
```

**Monitoring metrics:**
- User satisfaction/feedback
- Performance metrics
- Browser compatibility
- Feature request trends

---

## 9. Cost-Benefit Analysis

| Approach | Effort | Benefit | Risk | Timeline |
|---|---|---|---|---|
| Status Quo | 0 | 0 | Low | - |
| Leaflet + Plugins | Low | Medium-High | Very Low | 3-5 days |
| Upgrade to Maplibre | High | High | Medium | 2-3 weeks |
| Migrate to OpenLayers | High | Medium | Medium-High | 3-4 weeks |

---

## 10. Final Recommendation

### ✅ **RECOMMENDED: Leaflet + Smart Enhancements**

**Implementation Plan:**

```javascript
// Phase 1: Add Geocoder (Search)
// + Upgrade to better tile provider
// + Improve cluster visualization

// Phase 2: Optional Heatmaps
// + Show facility density
// + Category-based visualization

// Phase 3: Monitor & Feedback
// + Gather user feedback
// + Analyze usage patterns
// + Decide on Phase 3+ migration
```

**Why this approach:**
1. ✅ **Low Risk** - No breaking changes
2. ✅ **Quick Wins** - User sees improvement in days
3. ✅ **Progressive** - Can add features gradually
4. ✅ **Maintainable** - Team familiar with Leaflet
5. ✅ **Cost-Effective** - Minimal development effort
6. ✅ **Flexible** - Can upgrade to Maplibre later if needed

---

## Conclusion

**Current setup (Leaflet + CARTO) works fine**, tapi bisa di-enhance dengan:

1. **Search capability** → Geocoder
2. **Better visuals** → Esri/Stamen tiles
3. **More info** → Heatmap layers
4. **Better UX** → Fullscreen, improved clustering
5. **Future-proof** → Plan for Maplibre migration

**All enhancements are additive** - tidak perlu rewrite, hanya augment!

---

**Prepared:** June 23, 2026
**Next Review:** After Phase 1 implementation
