# Florida-Toxic-Release-Hotspots-2024
Interactive map of Florida Toxic Release Hotspots (TR)I 2024 facility releases using proportional circles, chemical filters, clustering, and a Space Coast zoom. It is built with Leaflet.

# Data Source and Sample
### Primary Dataset
 EPA Toxics Release Inventory (TRI) 2024 Preliminary, Basic Data Files, Florida subset is the primary dataset (downloaded data is in the assignment folder): https://www.epa.gov/toxics-release-inventory-tri-program/2024-tri-preliminary-dataset-basic-data-files
 These state-level CSV includes facility location, chemical identifiers, and quantities for on site releases and off site transfers. EPA confirms the 2024 preliminary files include forms processed as of July 9, 2025, and that “Basic” files contain the 100 most-used TRI fields in CSV format. According to the EPA’s data dictionary for the Basic files, the CSVs include ```YEAR, TRIFID (facility ID), FRS ID, FACILITY NAME, CITY, STATE, LATITUDE, LONGITUDE, PRIMARY NAICS, CHEMICAL```, and aggregated quantities such as ```TOTAL RELEASES and off site totals```. Coordinates come from EPA’s Facility Registry Service. 

### Geospatial Enrichment
Facility coordinates and crosswalks are governed by the Facility Registry Service (FRS). The FRS State Single File CSV is available for Florida and contains facility identifiers, geospatial fields, and program linkages. This is the authoritative reference used by TRI for location, which is why TRI LATITUDE and LONGITUDE are already reliable. 

### Cleaning Choices 
- Florida filter: kept rows where STATE == FL.
- Coordinates: kept rows where LATITUDE and LONGITUDE parsed to finite numbers within valid geographic ranges (lat −90 to 90, lon −180 to 180). Rows with missing, nonnumeric, or out-of-range coordinates were excluded.
- Numeric fields: Removed comma digit grouping from large values, for example 12,345 becomes 12345, then stored the results as numeric values. Applied to ```ON_SITE_RELEASE_TOTAL, OFF_SITE_RELEASE_TOTAL, and TOTAL_RELEASES```.
- Total logic: If TOTAL_RELEASES was missing or zero but either component had a value, set TOTAL_RELEASES to ON_SITE_RELEASE_TOTAL plus OFF_SITE_RELEASE_TOTAL.
- Column standardization: trimmed to these fields only to simplify the first map release: ```YEAR, TRIFID, FRS_ID, FACILITY_NAME, CITY, STATE, LATITUDE, LONGITUDE, PRIMARY_NAICS, CHEMICAL, ON_SITE_RELEASE_TOTAL, OFF_SITE_RELEASE_TOTAL, TOTAL_RELEASES```.
- Units: pounds for all quantities. TRI reports dioxin compounds in grams; those values remain as provided by EPA and will look comparatively small.

### Cleaned Dataset Summary 
- Saved as: ```data/tri_fl_2024_basic_clean.csv```
- Florida scope: 1,735 rows representing approximately 700 distinct facilities by TRIFID.
- Numbers: commas removed and converted to numeric types; totals calculated when missing.

# Topic and Geographic Phenomena
- What:
 Facility reported toxic chemical releases and transfers relevant to environmental health and exposure potential.
- Where:
 Florida statewide, with attention to clusters near the Space Coast as a use case.
- When:
Reporting year: 2024.
- Title:
Florida Toxics Release Hotspots 2024
- Subtitle:
 Mapping facility reported releases and transfers that matter for environmental health

### Thematic Methods
 - Proportional circle symbols sized by TOTAL_RELEASES
 - Distinct color per CHEMICAL so users can visually separate chemical - - types
 - Optional clustering at low zoom for legibility

### Anticipated UI
 - Chemical dropdown filter
 - Minimum release slider (pounds)
 - Text search for facility or city
 - Live counts for features shown and chemical diversity
 - Popup with facility details and on-site vs off-site split

# Map Objectives and User Needs
### Why this Map?
This TRI map establishes a Florida wide baseline of facility reported chemical releases that directly supports my dissertation on chemical contamination around spaceports. The map lets users see which facilities report which chemicals, where they are located, and visually inspect concentrations near the Space Coast focus area. Clear symbology, interactive filtering, and transparent units allow users to relate facility reports to potential pathways into water bodies and to compare these patterns with independent datasets on wastewater, microbial resistance, and aerospace activity.

### Intended Users and Needs
 - Residents and advocacy groups can use it to locate nearby facilities, see which chemicals are present, and judge amounts for health and advocacy decisions.
 - Journalists and local officials can use it to spot hotspots, verify sites, and move from statewide patterns to facility detail for reporting, planning, and enforcement.
 - To provide students and researchers with a documented and reproducible dataset for further analysis

# Technology Stack
- Data processing tools
 QGIS for quick inspection and field trimming when needed. The “clean” file is a flat CSV that preserves standardized quantities and coordinates described in US Enviromental Protection Agency (EPA) documentation. 
- Browser libraries
 Leaflet for the map, Papa Parse for CSV loading, Chroma.js for color generation, and Leaflet.markercluster for legibility when zoomed out.
