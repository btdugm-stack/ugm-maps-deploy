# Direktori Maps Fasilitas UGM

Paket ini dibuat dari file Excel `maps-ugm.xlsx`.

## Technology Stack

**Architecture:** Single-file static web app (zero-build, no bundler)

| Layer | Technology | Version |
|---|---|---|
| **Mapping** | Leaflet.js | 1.9.4 |
| **Clustering** | Leaflet.markercluster | 1.5.3 |
| **Basemap** | CARTO Light | - |
| **Data Format** | JSON + GeoJSON | - |
| **Frontend** | Vanilla JS (ES6+) | - |
| **Styling** | Inline CSS3 | - |

📄 **Lihat dokumentasi lengkap:** [STACK.md](STACK.md)

## 📚 Documentation

| Document | Description |
|---|---|
| **[STACK.md](STACK.md)** | Complete technology stack documentation |
| **[ARCHITECTURE.md](ARCHITECTURE.md)** | Visual architecture diagrams |
| **[QUICKREF.md](QUICKREF.md)** | Developer quick reference card |
| **[.github/copilot-instructions.md](.github/copilot-instructions.md)** | AI coding agent guidelines |

## Isi Folder
- `index.html` — UI peta berbasis Leaflet + OpenStreetMap.
- `facilities.json` — data fasilitas hasil ekstraksi dari Excel.
- `facilities.geojson` — data titik fasilitas dalam format GeoJSON.

## Cara Deploy di Laragon
1. Copy folder `ugm_maps_deploy` ke `C:/laragon/www/`.
2. Jalankan Laragon, aktifkan Apache/Nginx.
3. Buka browser: `http://localhost/ugm_maps_deploy/`.

## Cara Deploy di Hosting Statis
Upload tiga file utama ke hosting atau GitHub Pages:
- `index.html`
- `facilities.json`
- `facilities.geojson`

## Catatan Data
- Total record: 98
- Record dengan koordinat: 98
- Record tanpa koordinat: 0
- Koordinat diekstrak dari link Maps pada Excel, aplikasi tidak melakukan scraping Google Maps.
- Base map menggunakan OpenStreetMap.

## Scope Lokasi
- Kampus utama UGM: 96 titik
- Ekstensi/luar kampus utama: 2 titik
- Secara default UI menampilkan kampus utama; centang opsi "Tampilkan fasilitas ekstensi/luar kampus utama" untuk melihat seluruh titik.
