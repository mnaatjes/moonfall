# Explanation: Lunar Surface Coordinates and Elevation Standards

This document explains the geodetic systems and measurement baselines used to model the Moon's surface in the simulation.

---

## 1. Geodetic Coordinates (Latitude and Longitude)

A **Geodetic Coordinate** represents a unique location on the surface of the lunar sphere using a pair of angular measurements:

*   **Latitude (North-South):** Specifies the angle north or south of the lunar equator. 
    *   Ranges from **$-90.0^{\circ}$ (South Pole)** to **$+90.0^{\circ}$ (North Pole)**.
*   **Longitude (East-West):** Specifies the angle east of the Prime Meridian.
    *   Ranges from **$0.0^{\circ}$ to $360.0^{\circ}$**, measured continuously in the Eastern direction (following planetary cartography conventions).

---

## 2. Elevation, Altitude, and the Mean Radius Baseline

Because the Moon is not a perfect sphere, elevations are measured as offsets relative to a standardized average reference sphere:

### The Reference Datum
*   **Reference Value:** **$1737.4\text{ km}$ ($1,737,400\text{ meters}$)**.
*   **Source:** Established by the **International Astronomical Union (IAU)** Working Group on Cartographic Coordinates and Rotational Elements (WGCCRE). This is the standard vertical datum used in planetary GIS systems and NASA PDS datasets (such as LOLA elevation heightmaps).
*   **Calculation Method:** The Moon's shape is triaxial and slightly flattened. The $1737.4\text{ km}$ value represents the spherical mean (the average radius derived from the equatorial and polar measurements).

### Interpreting Elevation Values
*   **Positive Elevation ($> 0\text{ m}$):** Represents topography rising above the reference sphere (e.g., mountains, crater rims).
*   **Negative Elevation ($< 0\text{ m}$):** Represents topography dropping below the reference sphere (e.g., deep basins, crater floors).
