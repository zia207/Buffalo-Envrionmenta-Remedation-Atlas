# Greater Buffalo Remediation Dashboard — Code Guide

This document explains how `greater_buffalo_remediation_dashboard.html` works in plain language. Open that file in a browser to get an interactive map and tables about polluted and cleanup sites across Western New York — no server required.

---

## The Big Picture

The dashboard is a **single self-contained web page** (~2,400 lines). Everything runs in the browser:

| Part | Role |
|------|------|
| **HTML** | Layout — buttons, map box, tables, panels |
| **CSS** | Look and feel — colors, fonts, responsive layout |
| **JavaScript** | Logic — data, map, filters, clicks, exports |

External libraries loaded from CDNs:

- **Leaflet** — interactive maps
- **MarkerCluster** — groups nearby map markers when zoomed out
- **jsPDF / jspdf-autotable** — PDF export
- **SheetJS (XLSX)** — Excel export

---

## What Problem Does It Solve?

It combines **four environmental datasets** into one view:

| Code | Dataset | Description |
|------|---------|-------------|
| **NPL** | Federal Superfund | EPA National Priorities List sites (e.g. Love Canal) |
| **DEC** | NY State cleanup sites | Brownfields, State Superfund, Environmental Restoration, Voluntary Cleanup, RCRA (~941 sites) |
| **CEAM** | Chemical exposure | Facilities from EDF's Chemical Exposure Action Map |
| **CBS** | Chemical bulk storage | NYSDEC-registered bulk storage tanks |

Each site has a name, location (latitude/longitude), county, status, and other metadata. The page shows them on a map and in sortable tables.

---

## Where Does the Data Live?

Almost all data is **embedded in the HTML file** as large JavaScript objects:

- **`PAYLOAD`** — site records, county/ZIP boundary GeoJSON, place labels
- **`SVI_DATA`** — CDC Social Vulnerability Index scores by ZIP code

The file is large (~800 KB) because data is baked in rather than fetched from an API.

On startup, JavaScript normalizes everything into one list called **`RECORDS`**:

```javascript
// Simplified structure
RECORDS = [
  { key: 'npl-0', type: 'NPL', n: 'Site name', lat: 42.9, lng: -78.8, ... },
  { key: 'dec-0', type: 'DEC', ... },
  // ... 1,000+ total records
]
```

Field names are abbreviated (`n` = name, `co` = county) to keep the embedded JSON compact.

### Building `RECORDS`

| Source | Becomes |
|--------|---------|
| `PAYLOAD.npl` | Federal Superfund records |
| `PAYLOAD.remedial` | DEC cleanup records (program from `PROG_META`) |
| `PAYLOAD.ceam` | Chemical exposure records |
| `PAYLOAD.cbs` | Bulk storage records |

Each record also gets an **`inv`** (expanded chemical inventory) via `buildInventory()`, grouped into categories: PFAS, BFR, Pesticides, PPCPs, Legacy / industrial.

---

## Page Layout (Top to Bottom)

### 1. Header

Title, subtitle, UB RENEW logo — branding and context.

### 2. KPI Cards

Seven summary counters (e.g. "Records shown", "Federal Superfund", "DEC cleanup sites"). Updated whenever filters change via `renderKPIs()`.

### 3. Filters

Controls include:

- **Dropdowns:** county, DEC program, status, search box
- **SQL site selection:** guided filters for **city**, **town (Erie Co.)**, and **contaminant / chemical**, plus a **Custom SQL** mode for freeform `WHERE` clauses
- **Toggle chips:** turn datasets on/off (NPL, DEC, CEAM, CBS) and map layers (counties, cities, towns, ZIPs, SVI, labels, clustering)
- **DEC site picker:** optionally limit the map and PNG/PDF export to checked DEC cleanup sites by program

### 4. Map + Detail Panel

- **Left:** satellite map with colored markers, zoom controls, legend, optional SVI panel
- **Right:** "dossier" panel — full site details when you click a marker or table row

### 5. Charts

Horizontal bar charts built in plain HTML/CSS (recompute with filters):

- Sites by city
- Contaminant / inventory hits
- Sites by county
- DEC sites by program
- Cleanup status / site class
- Records by dataset

### 6. Tables

- **Site register** — all filtered records, sortable columns
- **CEAM table** — chemical facilities with release and health-risk data

### 7. Export Buttons

Download CSV, Excel, or a custom map as PNG/PDF. The **Export map** dialog lets you choose basemap, datasets, AOI, symbols, legend, and (via active filters) which **chemical / contaminant** sites appear on the figure.

### 8. About Data

Describes each dataset, sources, and record counts.

---

## Core Logic: `refresh()`

Most UI updates flow through one function:

```javascript
function refresh() {
  const recs = currentRecords();  // Apply filters
  renderKPIs(recs);               // Update KPI numbers
  renderMap(recs);                // Redraw map markers
  renderCharts(recs);             // Redraw bar charts
  renderTable(recs);              // Redraw main table
  renderCeam();                   // Redraw CEAM table
  updateInvCount();               // Update export stats
}
```

Changing any filter triggers `refresh()`, which redraws the whole UI to match.

---

## How Filtering Works

`passFilters(record)` decides whether a site is visible:

1. Its dataset type is enabled (`show.NPL`, `show.DEC`, etc.)
2. Optional DEC site-selection limit (`selectedSiteKeys`)
3. County matches (if selected)
4. DEC program matches (if selected)
5. Status matches: `active`, `done`, or `deleted` (via `statusGroup()`)
6. **Chemical / contaminant** (guided SQL) or a **custom SQL WHERE** clause — including `contaminant`, `inventory_category`, city, town, etc.
7. Search text appears in name, city, contaminants, or related fields

`currentRecords()` returns `RECORDS.filter(passFilters)`. The same filtered set feeds the map, KPIs, charts, tables, and **Export map (PNG/PDF)**.

---

## The Map (Leaflet)

### Basemap

Satellite imagery from Esri/Google OpenStreetMap, with automatic fallback if a tile source fails.

### Layer Stack

1. Satellite tiles (background)
2. County lines (optional)
3. ZIP boundaries **or** SVI choropleth (mutually exclusive)
4. Site markers (clustered or plain)
5. Text labels (cities, water bodies, county names)

### Marker Styles

| Type | Appearance |
|------|------------|
| NPL | Large red circles |
| DEC | Medium circles, color by program |
| CEAM | Purple circles |
| CBS | Small squares |

**Clustering:** when zoomed out, nearby markers merge into numbered bubbles (`L.markerClusterGroup`).

### Click Behavior

Clicking a marker or table row calls `selectRecord(key)`:

- Fills the detail panel
- Highlights the table row
- Pans/zooms the map to the site

---

## Detail Panel ("Dossier")

`selectRecord()` builds HTML based on site type:

| Type | Shows |
|------|-------|
| **NPL** | Status, listing year, contaminants, EPA notes |
| **DEC** | Program, site class, DEC site code, link to DER External Search |
| **CEAM** | Chemical, releases, health-risk bars, population within 10 km |
| **CBS** | Facility type, address, active registration info |

All types can show an **expanded chemical inventory** grouped by category.

---

## SVI Layer (Social Vulnerability Index)

CDC/ATSDR data on community vulnerability (poverty, housing, minority status, etc.).

- Each ZIP (ZCTA) is colored from blue (low) to red (high vulnerability)
- Metric selector: overall SVI, themes, or individual indicators
- Hover tooltips show percentile ranks (NY statewide)
- Toggle via the **SVI index** chip; disables ZIP boundary fill when active

Key objects: `SVI_DATA`, `SVI_CATALOG`, `sviByZip`, `sviLayer`.

---

## Exports

| Button | Output |
|--------|--------|
| Inventory CSV | One row per site × contaminant |
| Inventory Excel | Same data as `.xlsx` |
| CEAM CSV | Chemical facilities only |
| Full register CSV | Filtered site list |
| Export map (PDF/PNG) | Modal with options; renders map to canvas, saves via jsPDF or download |

### Export map (PDF / PNG)

Open **Export map (PDF / PNG)** from the map toolbar. The dialog redraws the figure on a canvas (basemap tiles, boundaries, markers, place labels, legend) and saves as PNG or PDF.

**Datasets (current filters apply)** — including chemical selection:

| Option | What it does |
|--------|----------------|
| Federal Superfund / DEC / CEAM / CBS checkboxes | Include or omit each dataset on the figure |
| **Select chemical / contaminant** | Use the dashboard **Contaminant** dropdown (or Custom SQL `contaminant` / `inventory_category` clause) before exporting — only sites matching that chemical appear on the PNG/PDF |
| Inventory categories | Choose a category such as **PFAS**, **BFR**, **Pesticides**, **PPCPs**, or **Legacy / industrial** to export sites linked to that chemical group |
| Only selected DEC cleanup sites | Limit DEC markers to sites checked in the program picker |

Because the modal states **current filters apply**, any active chemical, city, town, county, program, or status filter from the main dashboard is honored in the export. Typical workflow:

1. Select a **chemical** (e.g. PFAS, Dioxin, Trichloroethylene) under SQL site selection  
2. Confirm the map and summary cards show the intended sites  
3. Open **Export map** → choose basemap, AOI, symbols, and legend → download PNG or PDF  

Other export-map controls:

- **Basemap** — current view, satellite, streets, topography, or none  
- **Boundaries** — counties, Erie city/town outlines, ZIP lines, optional SVI choropleth  
- **Symbols** — shape, size, and colors for NPL / DEC / CEAM / CBS (DEC can use distinct program colors)  
- **Place labels** — font size and colors for readability on satellite imagery  
- **Legend** — position, font size, and which symbol groups to include  
- **AOI** — full region, core Buffalo, county, Erie city/town, or user-drawn bounding box  
- **Figure appearance** — title, data source line, and PDF figure size  

PDF export is the most complex feature — it redraws boundaries, markers, legend, and optionally a site register table on a canvas.

---

## Startup Sequence

At the bottom of the script:

```javascript
syncChips();      // Sync toggle button states
refresh();        // Draw everything
loadSviData();    // Index SVI data by ZIP
setTimeout(() => map.invalidateSize(), 200);  // Fix map sizing after layout
```

County and program dropdowns are populated from unique values in `RECORDS` during initialization.

Dataset counts in the About section are set from `PAYLOAD` lengths.

---

## Mental Model

```
Embedded data → unified RECORDS list → filters → refresh()
                                              ↓
                              map + charts + tables + KPIs
                                              ↓
                              click → detail dossier
```

**One sentence:** Embedded environmental data is normalized into a single list; filters decide what's visible; the map, charts, and tables all redraw together; clicking anything opens a detailed dossier.

---

## File Structure Cheat Sheet

| Lines (approx.) | Contents |
|-----------------|----------|
| 1–246 | CSS styling (`:root` variables, layout, responsive rules) |
| 248–760 | HTML structure |
| 762–766 | External script tags |
| 768+ | JavaScript |
| ~768 | `PAYLOAD` — sites + geography |
| ~1009 | `SVI_DATA` — vulnerability scores |
| ~775–819 | Metadata: `PROG_META`, `CLASS_MEANING`, `statusGroup()` |
| ~822–929 | Build `RECORDS` + chemical inventory |
| ~947–1005 | Map init, county/ZIP layers |
| ~1068–1263 | SVI layer logic |
| ~1295–1527 | Markers, filters, render functions |
| ~1530–1614 | Event listeners (filters, layers, zoom) |
| ~1616–1689 | CSV/Excel exports |
| ~1691–2381 | Map export (PDF/PNG) |
| ~2389–2393 | Initialize on load |

---

## Common Customization Points

| Goal | Where to look |
|------|----------------|
| Add or edit sites | Regenerate `PAYLOAD` (see build scripts elsewhere in the repo) |
| Change colors | CSS `:root` variables; `PROG_META`; `NPL_COLOR`, `CEAM_COLOR`, `CBS_COLOR` |
| Add a filter | Extend `passFilters()`, add HTML control, wire event listener → `refresh()` |
| Change default map view | `MAP_DEFAULT = { center: [42.98, -78.83], zoom: 10 }` |
| Adjust marker sizes | `makeMarker()` — radius values per type |

---

## Key Functions Reference

| Function | Purpose |
|----------|---------|
| `passFilters(r)` | Returns true if record passes all active filters |
| `recordMatchesContaminant(r, chem)` | Match a site to a selected chemical / inventory category |
| `currentRecords()` | Filtered list of records (also used by Export map) |
| `refresh()` | Master UI update |
| `renderMap(recs)` | Clear and redraw markers |
| `renderKPIs(recs)` | Update summary numbers |
| `renderCharts(recs)` | Update bar charts |
| `renderTable(recs)` | Update site register table |
| `renderCeam()` | Update CEAM table |
| `selectRecord(key)` | Show detail panel for one site |
| `makeMarker(rec)` | Create Leaflet marker for a record |
| `buildInventory(rec)` | Infer chemical categories for a site |
| `statusGroup(rec)` | Map record to active / done / deleted |
| `loadSviData()` | Index SVI CSV into `sviByZip` |
| `applySviVisibility()` | Show/hide SVI choropleth layer |

---

## Related Files

- `greater_buffalo_remediation_dashboard.html` — the dashboard itself
- `RENEW_logo.png` — header logo (referenced by the HTML)
- Build/data scripts in the repo (if present) — generate the embedded `PAYLOAD`
