# Reference: Lunar Datasets and Active Data Sources

This document acts as a data dictionary and directory tracking active scientific datasets of the Moon to construct game overlays. All data listed here is sourced from active planetary missions (primarily NASA’s Lunar Reconnaissance Orbiter, or LRO).

---

## 1. Topography, Slope, and Roughness (Elevation & Navigation Overlay)
*   **Property:** Physical terrain elevation, gradient slope (expressed in degrees), and terrain surface roughness.
*   **Active Mission/Instrument:** LRO - **LOLA (Lunar Orbiter Laser Altimeter)**.
*   **Data Products:** 
    *   `LDEM` (Lunar Digital Elevation Model) - 16-bit raster maps up to 512 pixels/degree.
    *   `GDRDSM` (Slope Maps) - Shows slope steepness to identify non-buildable regions.
*   **Primary Source:** [LOLA Geosciences Node](https://pds-geosciences.wustl.edu/missions/lro/lola.htm)
*   **How to Access:** Navigate to the LOLA GDR (Global Data Record) directories. Select the resolution corresponding to your scale (e.g., `ldem_128` or `ldem_256`) and download in `.IMG` or `.TIF` formats.

---

## 2. Albedo and Surface Imagery (Visual Map Overlay)
*   **Property:** Visual color map reflecting light reflectivity and mineral brightness.
*   **Active Mission/Instrument:** LRO - **LROC WAC (Wide Angle Camera)**.
*   **Data Products:** 
    *   `WAC Global Mosaic` - Multi-spectral and monochrome base mapping of the lunar surface.
*   **Primary Source:** [LROC Data Node](https://lroc.sese.asu.edu/data) / [Lunar Orbital Data Explorer](https://ode.rsl.wustl.edu/moon/)
*   **How to Access:** Search for "WAC Global Albedo Mosaic" on the LROC data portal. Download the mosaic matching your target resolution in GeoTIFF format.

---

## 3. Water Ice and Hydrogen Abundance (Resource Overlay)
*   **Property:** Concentration of hydrogen (water ice) inside Permanently Shadowed Regions (PSRs) at the poles.
*   **Active Mission/Instrument:** LRO - **LEND (Lunar Exploration Neutron Detector)** & Lunar Prospector (Neutron Spectrometer).
*   **Data Products:** 
    *   `Neutron Flux Maps` - Reduced neutron emissions correspond directly to elevated hydrogen levels.
*   **Primary Source:** [LEND PDS Data Viewer](https://geo.pds.nasa.gov/missions/lro/lend.htm)
*   **How to Access:** Access the LEND map repository to download polar spatial distribution maps representing epithermal neutron counts. Lower counts match blue "Water Ice" zones.

---

## 4. Titanium and Ilmenite Abundance (Construction Resource Overlay)
*   **Property:** Distribution of Titanium Dioxide ($TiO_2$) present in iron-rich basaltic *maria*.
*   **Active Mission/Instrument:** LRO - **LROC WAC (Wide Angle Camera)** multispectral band ratios.
*   **Data Products:** 
    *   `WAC TiO2 Abundance Map` - Derived by calculating UV-to-visible light ratios (321nm / 415nm).
*   **Primary Source:** [LROC Data Node / TiO2 Products](https://lroc.sese.asu.edu/)
*   **How to Access:** Download the global or regional TiO2 abundance maps. High concentrations (represented by values up to 10-15 wt%) represent optimal spots for mining ore.

---

## 5. Cosmic and Solar Radiation Levels (Hazard Overlay)
*   **Property:** Galactic cosmic ray (GCR) dosage rates and solar particle event (SPE) intensities.
*   **Active Mission/Instrument:** LRO - **CRaTER (Cosmic Ray Telescope for the Effects of Radiation)**.
*   **Data Products:** 
    *   `Calibrated RDR Data` - Records active radiation dosage rates in silicon and tissue-equivalent plastic.
*   **Primary Source:** [PDS Planetary Plasma Interactions (PPI) Node](https://pds-ppi.igpp.ucla.edu/)
*   **How to Access:** Locate the CRaTER archive inside the PPI Node. Select the historical timeline mapping solar cycles to extract radiation heatmaps mapped over coordinates.

---

## Summary Data Access Table
For generalized, fast downloads combining these layers, use the **Lunar Orbital Data Explorer**:

| Resource | Instrument | Primary Download Portal |
| :--- | :--- | :--- |
| **All Layers** | LRO Suite | [Lunar Orbital Data Explorer (ODE)](https://ode.rsl.wustl.edu/moon/) |
| **Topography** | LOLA | [USGS Astrogeology Search](https://astrogeology.usgs.gov/search) |
| **Visual Map** | LROC WAC | [LROC Data portal](https://lroc.sese.asu.edu/) |
| **Water Ice** | LEND | [PDS Geosciences LEND](https://geo.pds.nasa.gov/missions/lro/lend.htm) |
| **Radiation** | CRaTER | [PDS PPI Node](https://pds-ppi.igpp.ucla.edu/) |
