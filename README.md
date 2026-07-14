# Buffalo Environmental Remediation Atlas

Interactive, browser-based environmental atlases for Western New York and the Great Lakes basin. Each dashboard is a self-contained HTML file with embedded data — no build step, database, or backend required.

Developed in support of environmental remediation research at the [University at Buffalo RENEW Institute](https://www.buffalo.edu/renew.html) (Research and Education in Energy, Environment and Water).

---

## Live dashboards

| Atlas | File | Description |
|-------|------|-------------|
| **Greater Buffalo** | [`greater_buffalo_remediation_dashboard.html`](greater_buffalo_remediation_dashboard.html) | Federal Superfund, NYSDEC cleanup sites, chemical exposure facilities, and bulk storage across eight WNY counties |
| **Great Lakes Superfund** | [`great_lakes_superfund_dashboard.html`](great_lakes_superfund_dashboard.html) | EPA National Priorities List sites and Great Lakes Areas of Concern within the watershed boundary |

> **GitHub Pages:** If you enable Pages on this repository, `index.html` is served as the default entry point. The most current Buffalo atlas is `greater_buffalo_remediation_dashboard.html`; copy or symlink it to `index.html` if you want Pages to serve the latest version.

---

## Greater Buffalo Environmental Remediation Atlas

An interactive map and research register covering **Erie, Niagara, Cattaraugus, Chautauqua, Genesee, Wyoming, Orleans, and Allegany** counties.

### Datasets

| Layer | Records | Source |
|-------|---------|--------|
| Federal Superfund (NPL) | 6 | [EPA Superfund / CIMC](https://map22.epa.gov/cimc/) |
| NYSDEC cleanup sites | 941 | [DEC DER External Search](https://appfactory.dec.ny.gov/DERExternalSearch/) |
| Chemical Exposure Action Map | 16 | [EDF CEAM](https://ceam.edf.org/) (TRI-based) |
| Chemical Bulk Storage | 84 | NYSDEC Bulk Storage GIS (Erie & Niagara) |
| County boundaries | 8 | U.S. Census TIGER |
| ZIP code areas (ZCTA) | 212 | U.S. Census TIGER (simplified) |

DEC programs include State Superfund, Brownfield Cleanup, Environmental Restoration, Voluntary Cleanup, and RCRA corrective action.

### Features

- **Satellite basemap** (Esri World Imagery) with fallbacks and layer switcher (Google satellite, hybrid, OSM streets, Esri topo)
- **Filters** by county, DEC program, status/class, and free-text search
- **Toggleable datasets** — NPL, DEC, CEAM, CBS — with marker clustering
- **Boundary overlays** — county outlines and **distinct-color ZIP code polygons**
- **Site dossier panel** — click any marker or table row for program details, contaminants, and status
- **Chemical inventory** — expanded contaminant categories (PFAS, BFR, pesticides, PPCPs, legacy/industrial) with CSV and Excel export
- **Map export (PNG / PDF)** — configurable basemap, symbol shape/size/color, boundary line style, legend, resolution, and map extent
- **Register export** — filtered site table as CSV; optional register appendix in PDF exports

---

## Great Lakes Superfund Atlas

A companion atlas focused on the **Great Lakes watershed**, combining:

- EPA **National Priorities List** Superfund sites
- Great Lakes **Areas of Concern (AOCs)**
- Watershed boundary, state outlines, major tributaries, and lake labels
- Chemical inventory export and publication-quality **PNG / PDF map export** with extensive styling controls

---

## Quick start

### Open locally

Clone the repository and open either HTML file in a modern browser:

```bash
git clone https://github.com/zia207/Buffalo-Envrionmenta-Remedation-Atlas.git
cd Buffalo-Envrionmenta-Remedation-Atlas
```

**Option A — double-click** the HTML file in your file manager.

**Option B — local web server** (recommended for PDF/PNG export and tile loading):

```bash
python3 -m http.server 8080
```

Then visit:

- Buffalo atlas: [http://localhost:8080/greater_buffalo_remediation_dashboard.html](http://localhost:8080/greater_buffalo_remediation_dashboard.html)
- Great Lakes atlas: [http://localhost:8080/great_lakes_superfund_dashboard.html](http://localhost:8080/great_lakes_superfund_dashboard.html)

### Deploy to GitHub Pages

1. Push this repository to GitHub.
2. Go to **Settings → Pages**.
3. Set **Source** to deploy from the `main` branch (root `/`).
4. After deployment, the site will be available at  
   `https://<username>.github.io/Buffalo-Envrionmenta-Remedation-Atlas/`

To serve the latest Buffalo dashboard at the root URL, replace or update `index.html` with the contents of `greater_buffalo_remediation_dashboard.html`.

---

## Repository structure

```
Buffalo-Envrionmenta-Remedation-Atlas/
├── index.html                              # GitHub Pages default entry (Buffalo atlas)
├── greater_buffalo_remediation_dashboard.html  # Greater Buffalo atlas (canonical)
├── great_lakes_superfund_dashboard.html    # Great Lakes Superfund & AOC atlas
├── RENEW_logo.png                          # University at Buffalo RENEW logo
└── README.md
```

All site data, county/ZIP GeoJSON, and application logic are embedded directly in each HTML file. There are no separate data files to maintain for the Buffalo atlas.

---

## Technology

- **[Leaflet](https://leafletjs.com/)** — interactive mapping
- **[Leaflet.markercluster](https://github.com/Leaflet/Leaflet.markercluster)** — marker clustering (Buffalo atlas)
- **Esri / OpenStreetMap / Google** — basemap tiles (network access required)
- **[jsPDF](https://github.com/parallax/jsPDF)** + **jsPDF-AutoTable** — PDF report generation
- **[SheetJS (xlsx)](https://sheetjs.com/)** — Excel export (loaded from CDN)

---

## Data notes

- NYSDEC coordinates were converted from **UTM Zone 18N (NAD83)** to WGS84 latitude/longitude for web display.
- Chemical inventory associations are **screening-level** inferences from EPA NPL records, DEC program typology, TRI chemical identity, and emerging-contaminant literature — not confirmatory sampling results.
- Program classifications and cleanup status change over time. **Confirm any specific site against official [DEC](https://appfactory.dec.ny.gov/DERExternalSearch/) and [EPA](https://map22.epa.gov/cimc/) databases** before relying on this atlas for decisions.
- The NYC OER cleanup dataset applies to New York City only and is intentionally excluded from the Buffalo atlas.

---

## Disclaimer

These atlases are **unofficial research compilations** for education and exploratory analysis. They are provided as-is without warranty. Basemap tiles, export features, and external CDN libraries require an internet connection when viewing or generating figures.

---

## Acknowledgments

- **University at Buffalo — RENEW Institute**
- **U.S. EPA** — Superfund NPL and CIMC data
- **New York State DEC** — Environmental Remediation and Bulk Storage databases
- **Environmental Defense Fund** — Chemical Exposure Action Map
- **U.S. Census Bureau** — TIGER/ZCTA boundary geometry
