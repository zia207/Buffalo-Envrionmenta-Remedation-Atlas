# NSF Proposal — Project Description Section

**Project title:** Greater Buffalo Environmental Remediation Atlas: An Open Geospatial Platform for Contaminated-Site Inventory, Chemical Exposure Screening, and Environmental Justice Analysis in Western New York

**Institution:** University at Buffalo — RENEW Institute (Research and Education in Energy, Environment and Water)

**Use:** Insert this document as the **Project Description** section of the NSF proposal. Principal investigator, co-investigators, senior personnel, budget, facilities, and data-management plan supplements will be added separately in the full submission package.

**Prototype:** Functional open-source dashboard — [Buffalo-Envrionmenta-Remedation-Atlas](https://github.com/zia207/Buffalo-Envrionmenta-Remedation-Atlas)

---

## 1. Introduction and Motivation

Western New York (WNY) occupies a singular place in U.S. environmental history. The Love Canal disaster in Niagara Falls directly led to the 1980 Comprehensive Environmental Response, Compensation, and Liability Act (Superfund). Today the eight-county region—Erie, Niagara, Cattaraugus, Chautauqua, Genesee, Wyoming, Orleans, and Allegany—still carries that legacy: federal National Priorities List sites, hundreds of state-tracked remediation properties, ongoing toxic chemical releases reported to EPA's Toxics Release Inventory, and regulated hazardous chemical bulk storage along the Niagara River, Buffalo River, and Lake Erie shoreline.

Public information describing these sites **exists**, but it is **fragmented**. EPA Superfund profiles, NYSDEC Environmental Remediation databases, third-party health-risk screening tools, and CDC Social Vulnerability Index (SVI) data live in separate portals with incompatible coordinate systems, program-specific vocabularies, and no unified geography for Western New York. Researchers, planners, community organizations, and residents therefore lack a single, reproducible baseline for asking basic questions: *What remediation and chemical-release activity occurs in this ZIP code? How does it relate to neighborhood vulnerability? Which emerging contaminants—PFAS, 1,4-dioxane, pesticides, pharmaceuticals—warrant further study?*

This project proposes to advance and sustain the **Greater Buffalo Environmental Remediation Atlas**, a working geospatial research platform that already integrates **1,047 georeferenced site and facility records** from five authoritative public sources with ZIP-level SVI themes and indicators, interactive visualization, **population-at-risk analysis by ZIP code**, chemical-inventory export, and publication-quality map generation. NSF support will transform a functional prototype into validated, updatable cyberinfrastructure for environmental health and environmental justice research in the Great Lakes basin.

---

## 2. Intellectual Merit

The proposed work advances knowledge and methods at the intersection of **environmental data science**, **contaminated-site geography**, and **environmental justice**.

### 2.1 Scientific gap

No existing public product integrates, for Western New York:

- EPA Superfund (NPL) sites  
- NYSDEC cleanup programs (State Superfund, Brownfield Cleanup, Environmental Restoration, Voluntary Cleanup, RCRA corrective action)  
- Health-risk–screened toxic release facilities (EDF Chemical Exposure Action Map)  
- Active chemical bulk storage (NYSDEC CBS)  
- CDC/ATSDR SVI at the ZCTA level with theme- and indicator-level selection  

The intellectual contribution is a **reproducible integration methodology**—coordinate harmonization, schema unification, provenance documentation, and rule-based chemical-inventory inference—that other post-industrial regions can adapt.

### 2.2 Research questions

1. How can heterogeneous federal and state remediation databases be harmonized into a single, auditable regional inventory without proprietary GIS infrastructure?  
2. To what extent do remediation sites and chemical-release facilities co-locate with census-defined social vulnerability across WNY ZIP codes?  
3. How can interactive, ZIP-level analysis tools quantify **population at risk** by combining SVI themes and indicators with contamination-site density, program status, and inferred pollutant classes?  
4. Can screening-level chemical inventories that tag PFAS, brominated flame retardants, pesticides, and PPCPs—inferred from program typology, NPL records, TRI identity, and NYSDEC sampling guidance—support prioritization of emerging-contaminant research?  
5. Does a static, browser-based atlas architecture provide sufficient accessibility and reproducibility for community-facing environmental data products?

### 2.3 Methodological advances

**Unified geospatial schema.** Site records from four program types (NPL, DEC, CEAM, CBS) are normalized to a common attribute model (location, county, ZIP, program, status class, contaminants, source metadata). NYSDEC coordinates in UTM Zone 18N (NAD83) are transformed to WGS84 for web display—a generalizable pipeline for state databases that do not natively export latitude/longitude.

**SVI–site overlay framework.** The atlas implements CDC/ATSDR SVI 2022 at the ZCTA level with the same theme and indicator structure as the federal [SVI Interactive Map](https://www.atsdr.cdc.gov/place-health/php/svi/svi-interactive-map.html): Overall SVI; four themes (socioeconomic status; household characteristics; racial and ethnic minority status; housing type and transportation); and 20+ indicators including below poverty, unemployment, disability, and minority population share. This enables systematic EJ hypothesis testing (e.g., disproportionate siting of active remediation or bulk storage in high-SVI areas).

**Screening chemical inventory.** Beyond source-database contaminant lists, the platform applies documented inference rules to assign chemicals to categories (PFAS, BFR, pesticides, PPCPs, legacy/industrial), including NYSDEC Part 375 screening analytes (PFAS, 1,4-dioxane) for all DEC sites. Methods will be published for reproducibility and critique.

**Open, static-first cyberinfrastructure.** The prototype embeds data in a self-contained web application (HTML/JavaScript/Leaflet) requiring no server-side database—lowering barriers to hosting, inspection, and fork-based replication. NSF support will refactor toward versioned GeoJSON/JSON bundles with automated update scripts while preserving transparency.

### 2.4 Preliminary results (prototype completed)

A functional **Greater Buffalo Environmental Remediation Atlas** demonstrates feasibility:

| Layer | Records | Source |
|-------|---------|--------|
| Federal Superfund (NPL) | 6 | EPA CIMC |
| NYSDEC cleanup sites | 941 | DEC DER External Search |
| Chemical Exposure Action Map | 16 | EDF (TRI-based) |
| Chemical Bulk Storage | 84 | NYSDEC CBS (Erie & Niagara) |
| County / ZCTA boundaries | 8 / 212 | U.S. Census TIGER |
| SVI (WNY ZCTAs) | 217 | CDC/ATSDR SVI 2022 |

Implemented capabilities include: multi-dataset satellite map with clustering; filters by county, program, status, and keyword; SVI choropleth with themes/indicators; site dossier panel; dynamic summary charts; chemical inventory export (CSV/Excel); and configurable PNG/PDF map export for publications.

A companion **Great Lakes Superfund & Areas of Concern atlas** in the same repository demonstrates extensibility of the architecture to basin-scale questions.

These preliminary results reduce technical risk and allow the funded project to focus on validation, automation, evaluation, and community integration rather than proof-of-concept development.

### 2.5 Mapping Population at Risk: Interactive SVI–Contamination Analysis by ZIP Code

Environmental justice research requires more than static maps of where sites and vulnerable populations are located separately. Communities, planners, and researchers need **interactive tools** that answer, for any Western New York ZIP code (ZCTA): *How many people live in neighborhoods with high social vulnerability **and** measurable contamination burden? Which pollutant classes and remediation programs dominate? How does this ZIP compare to county and regional baselines?*

#### 2.5.1 Conceptual framework

**Population at risk** in this project is defined operationally—not as individual medical risk, but as **census-tract–aggregated population residing in ZCTAs where elevated social vulnerability (SVI) intersects with documented contamination activity**. The atlas links:

| Data dimension | Role in population-at-risk mapping |
|----------------|--------------------------------------|
| **SVI (CDC/ATSDR 2022)** | ZIP-level percentile ranks for Overall SVI, four themes, and 20+ indicators (poverty, unemployment, disability, minority share, crowded housing, etc.) |
| **Remediation & release sites** | Point counts and attributes by program (NPL, DEC cleanup, CEAM, CBS) within each ZCTA |
| **Contaminant/pollutant classes** | Screening-level chemical tags (PFAS, legacy solvents, pesticides, PPCPs, BFR) inferred from site records |
| **Population denominators** | Census population (`E_TOTPOP`) embedded in SVI extracts for exposure-weighted summaries |

The intellectual contribution is a **reproducible overlay methodology** that moves from visual co-location to **quantified, exportable ZIP profiles** suitable for hypothesis testing, grant applications, and community testimony.

#### 2.5.2 Interactive analysis tools (prototype and proposed extensions)

The working atlas already supports foundational SVI–site overlay:

- **SVI choropleth layer** with CDC-aligned theme and indicator selector (Overall SVI; socioeconomic status; household characteristics; racial/ethnic minority status; housing and transportation)  
- **Statewide percentile ranking** for WNY ZCTAs, consistent with the federal [SVI Interactive Map](https://www.atsdr.cdc.gov/place-health/php/svi/svi-interactive-map.html)  
- **Simultaneous display** of remediation, release, and bulk-storage point layers against ZIP boundaries  
- **Hover and dossier panels** linking individual sites to county, ZIP, program, status, and contaminant fields  

NSF support will extend these capabilities into a dedicated **Population at Risk** analysis module:

| Tool | Function |
|------|----------|
| **ZIP profile panel** | Click or search a ZCTA to view SVI theme/indicator ranks, total population, site counts by program, active vs. closed status mix, and dominant pollutant classes |
| **Contamination burden index** | Rule-based composite score per ZIP combining site density, presence of NPL/SSF sites, CEAM health-risk facilities, and CBS storage—stratified by pollutant category |
| **SVI × pollution cross-tabulation** | Classify ZCTAs into quadrants (e.g., high SVI + high site density) with population-weighted totals for the region |
| **Comparative ranking table** | Sort all WNY ZIPs by selected SVI indicator, site count, or composite burden; export to CSV/Excel |
| **Buffer and proximity analysis** | Optional 0.5 km, 1 km, and 2 km buffers around sites to flag SVI-ranked populations near active remediation or storage |
| **Dynamic filters** | Restrict analysis to PFAS-tagged sites, specific counties, or program types while updating ZIP-level summaries in real time |
| **Publication exports** | Configurable PNG/PDF maps showing SVI choropleth with site overlays and legend annotations for hearings and manuscripts |

All metrics will carry **provenance metadata** (source, retrieval date, inference rule version) and explicit disclaimers that SVI describes population-level vulnerability rankings, not individual health outcomes.

#### 2.5.3 Research applications

The ZIP-level analysis module will support funded research on:

1. **Disproportionate burden** — Are high-SVI ZCTAs over-represented among ZIPs with the highest remediation-site density or PFAS-inferred inventory?  
2. **Program-type patterns** — Do state cleanup programs (BCP, SSF, VCP) cluster differently relative to SVI than federal NPL sites or TRI-based CEAM facilities?  
3. **Emerging contaminants** — Which high-vulnerability ZIPs combine PFAS/1,4-dioxane screening flags with active remediation status?  
4. **Temporal change** — As quarterly releases update site status, how do population-at-risk totals shift across the eight-county region?  

Methods and validation (comparison with published EJ indices, sensitivity to buffer distance and SVI theme choice) will be documented in the peer-reviewed methods publication under Aim 2.

#### 2.5.4 Illustrative use cases

- A community organization selects Overall SVI and exports a ranked list of ZIP codes with ≥ 5 active DEC sites and Overall SVI above the 75th statewide percentile for a public hearing packet.  
- A graduate researcher cross-filters CEAM facilities reporting PFAS releases against ZCTAs in the top quartile for minority population share and housing-cost burden.  
- A regional planner generates a PDF map of Erie County showing SVI socioeconomic theme with CBS bulk-storage points to inform brownfield prioritization near the Buffalo River corridor.  

These workflows demonstrate how interactive SVI–contamination analysis transforms fragmented agency data into actionable, ZIP-scaled intelligence about **who may be most affected** by legacy and ongoing pollution in Western New York.

---

## 3. Broader Impacts

NSF's broader-impacts criterion is central to this project. The atlas is designed not only to advance research but to **democratize access** to complex environmental data in a region with deep environmental-justice concerns.

### 3.1 Benefit to society

| Audience | Impact |
|----------|--------|
| **Residents** | Identify remediation sites, releases, and storage near homes and schools; view ZIP-level vulnerability context and population-at-risk summaries |
| **Community & EJ organizations** | Export ranked ZIP profiles, cross-tabulation tables, and maps for public hearings, grant applications, and advocacy |
| **Local & regional planners** | See contamination patterns across county boundaries; prioritize brownfields in high-SVI, high-burden ZIPs |
| **Legal aid & public health** | Link site data and pollutant classes to SVI indicators for equity-focused casework, exposure study design, and needs assessment |
| **K–12 and public science** | Case studies (Love Canal, Buffalo River, Tonawanda) tied to living regional data |

The platform complements—not replaces—official EPA and NYSDEC systems, with clear disclaimers directing users to authoritative databases for regulatory decisions.

### 3.2 Education and workforce development

The atlas will be integrated into graduate and undergraduate coursework at the University at Buffalo in GIS, environmental health, and Great Lakes science. Training workshops (researchers, planners, community advocates) and recorded tutorials will build geospatial and data-literacy skills without requiring commercial GIS licenses.

Undergraduate and graduate researchers will participate in data QA, usability testing, and case-study development—training the next generation in open environmental data science.

### 3.3 Dissemination and open science

All code, documented update scripts, and redistribution-compliant datasets will be released openly (license pending institutional review). Methods for chemical-inventory inference and coordinate harmonization will be submitted as a technical report and peer-reviewed publication. The platform supports NSF's goals of **reproducible research** and **public access to federally funded outcomes**.

### 3.4 Ethical engagement

An advisory group including community-based organizations will review UI language, workshop materials, and acceptable-use guidance to prevent stigmatization of neighborhoods based on SVI scores and to emphasize remediation as a community benefit. EJ communities will co-design story-map narratives for major WNY sites.

### 3.5 Alignment with RENEW Institute and Great Lakes research

The University at Buffalo RENEW Institute integrates energy, environment, and water scholarship. This project connects contaminated-site geography to Great Lakes tributary systems (Niagara, Buffalo, Cattaraugus, Chautauqua watersheds) and supports NSF priorities in environmental sustainability, cyberinfrastructure, and STEM education.

---

## 4. Proposed Research Plan

The three-year plan progresses from **data validation and automation** through **platform expansion and evaluation** to **sustainability and transferability**.

### 4.1 Aim 1 — Validated, automated regional data infrastructure

**Objective:** Replace manual compilation with reproducible ingestion, QA, and quarterly release cycles.

**Tasks:**
- Develop Python pipelines for EPA CIMC, NYSDEC DER, EDF CEAM, NYSDEC CBS, Census TIGER/ZCTA, and CDC SVI sources  
- Implement coordinate QA (target: locational uncertainty documented for ≥ 99% of records)  
- Produce FGDC/ISO 19115–aligned metadata and a public data dictionary  
- Release versioned dataset snapshots with changelogs  
- Conduct legal review of redistribution terms for each source  

**Expected outcome:** Documented, citable regional inventory updated on a fixed schedule; technical report on harmonization methodology.

### 4.2 Aim 2 — Platform hardening and scientific extensions

**Objective:** Refactor the prototype for maintainability; add layers and analyses that support new research questions.

**Tasks:**
- Separate data (GeoJSON/JSON) from presentation layer  
- Add spill and groundwater remediation layers from NYSDEC open data where available  
- Implement watershed clip filters (Buffalo River, Niagara River, Lake Erie drainage)  
- **Build Population at Risk module:** ZIP profile panel, contamination burden index, SVI × pollution cross-tabulation, comparative ranking exports, and configurable buffer proximity analysis  
- Validate population-at-risk metrics against independent EJ screening tools and publish sensitivity analysis (SVI theme choice, buffer distance, site-status filters)  
- Extend chemical-inventory rules with formal sensitivity analysis  
- Conduct WCAG 2.1 accessibility audit; improve mobile layout  
- Harmonize codebase with Great Lakes Superfund/AOC companion atlas  

**Expected outcome:** Production-grade open platform; peer-reviewed methods paper on SVI–site co-location and emerging-contaminant screening.

### 4.3 Aim 3 — Community engagement, evaluation, and education

**Objective:** Measure usability and impact; embed the atlas in research and community practice.

**Tasks:**
- Establish stakeholder advisory group (two meetings per year)  
- Deliver three training workshops with publicly archived materials  
- Conduct usability study (≥ 20 participants: researchers, planners, community users)  
- Develop narrative case studies (Love Canal, Tonawanda, Bethlehem Steel corridor)  
- Integrate atlas modules into UB courses; support ≥ 4 student research projects  
- Track metrics: sessions, exports, workshop attendance, citations  

**Expected outcome:** Evaluation report; demonstrated use in community and classroom settings; story-map educational resources.

### 4.4 Aim 4 — Sustainability and regional transferability

**Objective:** Ensure the atlas persists beyond the award period and serves as a template for other regions.

**Tasks:**
- Deploy on institutional hosting with documented SLA  
- Deposit datasets and software in UB research archive with DOI  
- Publish contributor guide and "build your own regional atlas" documentation  
- Identify follow-on funding pathways (state EJ programs, EPA, foundations) in written sustainability plan  

**Expected outcome:** Stable public URL; archived artifacts; replication guide for other Rust Belt / Great Lakes communities.

### 4.5 Timeline (three-year summary)

| Period | Focus |
|--------|--------|
| **Year 1** | Automated pipelines; first quarterly releases; data dictionary; refactor data/presentation split |
| **Year 2** | Population at Risk module (ZIP profiles, burden index, cross-tabs); new layers (spills/groundwater); accessibility; usability study; workshops; methods publication |
| **Year 3** | Institutional hosting; archive deposit; case studies; sustainability plan; final evaluation |

---

## 5. Evaluation and Success Metrics

| Metric | Target (Year 3) |
|--------|-----------------|
| Quarterly dataset releases completed | ≥ 10 |
| Coordinate QA coverage | ≥ 99% of records documented |
| Unique annual web sessions | ≥ 5,000 |
| Data exports (CSV, Excel, PNG, PDF) | ≥ 500 cumulative |
| Workshop participants | ≥ 75 |
| Usability task completion | ≥ 85% |
| ZIP-level population-at-risk profile exports | ≥ 200 cumulative |
| Student research projects using atlas | ≥ 4 |
| Peer-reviewed or technical publications | ≥ 2 |
| External citations or media references | ≥ 3 |

Qualitative evaluation (semi-structured interviews with planners and community partners) will assess whether the atlas informed testimony, site prioritization, or research design.

---

## 6. Data Management (summary)

*A full NSF Data Management Plan will accompany the complete proposal.*

- **Products:** Versioned site/facility GeoJSON, SVI extracts, update scripts, data dictionary, archived HTML dashboard releases  
- **Storage:** GitHub (development); UB institutional repository (long-term archive with DOI)  
- **Access:** Public, open redistribution for all compiled fields derived from public sources  
- **Retention:** Minimum five years post-award in UB archive; ongoing hosting subject to sustainability plan  
- **Provenance:** Source URL, retrieval date, and transformation log for every release  

---

## 7. Limitations and Responsible Use

The project will communicate limitations transparently in the atlas interface and documentation:

- Chemical inventory assignments are **screening-level inferences**, not laboratory results.  
- Site status changes; users must verify against official [DEC](https://appfactory.dec.ny.gov/DERExternalSearch/) and [EPA CIMC](https://map22.epa.gov/cimc/) records before regulatory or legal use.  
- SVI describes **population-level** vulnerability rankings, not individual health risk.  
- CBS coverage is limited to Erie and Niagara counties; petroleum-only storage is excluded by design.  

---

## 8. Expected Deliverables

1. Production **Greater Buffalo Environmental Remediation Atlas** with quarterly updates  
2. Open-source repository with automated ingestion, QA logs, and contributor documentation  
3. Technical report and peer-reviewed publication on integration methodology, SVI–contamination overlay, and population-at-risk ZIP analysis  
4. Usability and impact evaluation report  
5. Workshop curricula and case-study story maps  
6. Archived datasets with DOI via University at Buffalo  
7. Replication guide for other regions  

---

## 9. Conclusion

Western New York's environmental history demands tools that match the complexity of its contamination legacy. The Greater Buffalo Environmental Remediation Atlas converts scattered public records into an integrated, map-based research platform linking remediation sites, chemical releases, bulk storage, and social vulnerability—with interactive tools to map **population at risk** at the ZIP-code scale by combining SVI indicators and contamination-site pollution profiles. These are questions at the heart of NSF's commitment to advancing knowledge and benefiting society.

Preliminary prototype development demonstrates technical feasibility. NSF funding will provide the validation, automation, evaluation, and dissemination needed to establish this platform as durable cyberinfrastructure for Great Lakes environmental health and environmental justice research, education, and public engagement.

---

## Appendix — Data Sources

| Source | URL | Proposed refresh |
|--------|-----|------------------|
| EPA Superfund / CIMC | https://map22.epa.gov/cimc/ | Quarterly |
| NYSDEC DER External Search | https://appfactory.dec.ny.gov/DERExternalSearch/ | Quarterly |
| EDF Chemical Exposure Action Map | https://ceam.edf.org/ | Semi-annual |
| NYSDEC Bulk Storage GIS | NYSDEC open data | Semi-annual |
| CDC/ATSDR SVI | https://www.atsdr.cdc.gov/place-health/php/svi/ | Upon CDC release |
| U.S. Census TIGER / ZCTA | https://www.census.gov/geographies/mapping-files.html | As needed |

## Appendix — Prototype Capability Matrix

| Capability | Status |
|------------|--------|
| Multi-dataset map (NPL, DEC, CEAM, CBS) | Complete |
| Eight-county WNY extent | Complete |
| County and ZIP boundaries | Complete |
| CDC/ATSDR SVI themes & indicators | Complete |
| SVI choropleth + site overlay (visual co-location) | Complete |
| ZIP profile panel & population-at-risk metrics | Proposed (Year 2) |
| Contamination burden index & SVI × pollution cross-tab | Proposed (Year 2) |
| Buffer proximity analysis (0.5–2 km) | Proposed (Year 2) |
| Site dossier and searchable register | Complete |
| Chemical inventory export (CSV / Excel) | Complete |
| PNG / PDF map export | Complete |
| Automated quarterly refresh | Proposed |
| Formal usability evaluation | Proposed |
| Additional regulatory layers | Proposed |

---

*Draft for NSF Project Description — July 2026*  
*University at Buffalo, RENEW Institute*  
*PI, co-PIs, budget, facilities, and full Data Management Plan — to be added to complete proposal package.*
