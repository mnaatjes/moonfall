# Explanation: Lunar Surface Coordinates and Elevation Standards

This document explains the geodetic systems and measurement baselines used to model the Moon's surface in the simulation.

---

## 1. Geodetic Coordinates (Latitude and Longitude)

A **Geodetic Coordinate** represents a unique location on the surface of the lunar sphere using a pair of angular measurements:

*   **Latitude (North-South):** Specifies the angle north or south of the lunar equator. 
    *   Ranges from **$-90.0^{\circ}$ (South Pole)** to **$+90.0^{\circ}$ (North Pole)**.
*   **Longitude (East-West):** Specifies the angle east of the Prime Meridian.
    *   Ranges from **$0.0^{\circ}$ to $360.0^{\circ}$**, measured continuously in the Eastern direction (following planetary cartography conventions).

### Simulation Integration
The Geodetic Coordinate system is the **foundational coordinate reference** for the simulation, not just a visual overlay:

*   **Database Mapping:** Heightmap datasets (NASA LOLA DEM) and resource maps are stored and indexed by Latitude and Longitude.
*   **Gameplay Math:** Launch vehicle trajectories, satellite orbits, and ground locations are calculated using geodetic math.
*   **Unity Conversion:** When displaying the Moon in Unity, a mathematical projection converts these geodetic coordinates into 3D Cartesian coordinates (`Vector3` vectors) to render the 3D lunar sphere.

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

---

## 3. Global Partitioning and Sectoring Methodologies

To divide a planetary sphere into discrete playable sectors or data grids, cartographers and simulation engineers use several primary partitioning systems:

### A. Graticules
*   **Definition:** The network of intersecting lines of latitude and longitude mapped onto a globe.
*   **Role:** Serves as the primary coordinate grid for standard planetary navigation and alignment.

### B. Bounding Using Four Coordinates (Quadrangles)
*   **Definition:** Specifying a rectangular geographic area on a spherical grid using four values: Minimum Latitude ($Lat_{min}$), Maximum Latitude ($Lat_{max}$), Minimum Longitude ($Lon_{min}$), and Maximum Longitude ($Lon_{max}$).
*   **Role:** Used by USGS and NASA to divide the Moon into standardized cartographic sheets.
*   **Limitations:** Near the poles, the physical width of longitude steps approaches zero, leading to severe geometric distortion and shrinking sector sizes.

### C. CubeSphere and Cube Mapping
*   **Definition:** Projecting a spherical surface onto the six flat faces of an enclosing cube. Each flat face is then subdivided using a regular two-dimensional grid (often managed recursively via a Quadtree structure).
*   **Role:** Eliminates the polar singularity (where coordinate lines converge at a single point) and ensures relatively uniform grid shapes for physics and terrain rendering.

### D. Geodesic Grid
*   **Definition:** A planetary grid constructed by projecting a regular polyhedron (usually an icosahedron with 20 faces) onto the surface of a sphere.
*   **Role:** Subdivides the sphere into a network of triangles or hexagons that minimize spatial distortion.

### E. Discrete Global Grid Systems (DGGS)
*   **Definition:** A standardized spatial reference framework that uses a hierarchical tessellation of equal-area cells to cover the entire Earth or Moon.
*   **Role:** Ensures that every cell at a given resolution level represents the exact same physical area on the ground, which is essential for accurate resource and environment calculations.

---

## 4. Key Differences: Latitude/Longitude Grid vs. CubeSphere

While both systems represent points on a spherical Moon, they solve different mathematical and rendering challenges:

| Feature | Latitude/Longitude Grid (Spherical) | CubeSphere (Cube-Projected) |
| :--- | :--- | :--- |
| **Grid Lines** | Formed by concentric rings (latitude) and pole-to-pole lines (longitude). | Formed by standard 2D Cartesian grids on 6 flat cube faces. |
| **Polar Behavior** | Longitude lines converge at the North and South poles (singularities). | The poles are treated like ordinary surface points on the cube faces. |
| **Cell Distortion** | High distortion; cells stretch into narrow triangles near the poles. | Low distortion; cells remain relatively square across the entire surface. |
| **Primary Use** | Standardized human navigation and GIS coordinate references. | Game engine rendering, physics simulation, and real-time terrain level-of-detail (LOD) calculations. |

---

## 5. Game-World Simulation Modeling and Coordinates

The game-world is modeled as a spherical globe divided into geographic coordinate quadrangles (Sectors). 

Here is how position, size, and collision boundaries are represented in the Model:

### A. Position Representation
*   **Inside a Sector (Local Play):** Position is represented using Local Metric Offsets (meters $(x,z)$ from the southwest corner of the loaded Sector).
*   **Global (Launch/Orbit):** Position is represented using Geodetic Coordinates (Latitude, Longitude) on the spherical globe.

### B. Size and Collision Boundaries
*   **Local Level:** Collision bounds and building sizes are represented in meters (e.g., a $20\text{ m} \times 20\text{ m}$ collision box).
*   **Why this works:** When a Sector is loaded, it is treated as a flat, local metric plane. This allows standard Unity physics and collision logic to work seamlessly in meters without dealing with complex spherical curves during active play.

### C. The Role of the CubeSphere
If the game-world is modeled as a sphere, the CubeSphere is utilized for the following:
*   The CubeSphere is used as a database index and streaming system. It is a way to divide the spherical Moon into a quadtree grid so the game knows which sectors to load/unload from the hard drive as the camera moves, avoiding the math errors that happen when using lat/lon grids at the poles.

---

## 6. Game-World Coordinate Hierarchy

### A. Local Metric Coordinate (`LocalOffset`)
*   **Scale:** Micro Scale (Inside a single playable Sector).
*   **Definition:** A flat, 2D coordinate $(x,z)$ measured in meters from a Sector's designated origin point (the southwest corner, which represents $(0,0)$).
*   **Example:** Placing a Solar Panel at local position $(x = 45.0\text{ m}, z = 120.5\text{ m})$ relative to the bottom-left corner of Sector `SEC-402`.

### B. Global Geodetic Coordinate (`GeoCoordinate`)
*   **Scale:** Macro Scale (The entire lunar globe).
*   **Definition:** An angular coordinate pair (Latitude, Longitude) in degrees representing a physical point on the spherical Moon.
*   **Example:** The Shackleton Crater (a key resource site at the South Pole) is located at:
    *   `Latitude = -89.9°`
    *   `Longitude = 0.0°`

### C. Unity Engine Coordinate (`Vector3` / World Position)
*   **Scale:** View Scale (Rendering and physics engine).
*   **Definition:** A 3D Cartesian vector $(X, Y, Z)$ in Unity world space, representing the virtual position of a rendered GameObject inside the virtual environment volume.
*   **Architectural Scope (MVP Boundary):** These coordinates are strictly utilized in the **View** and **Presenter** layers of the Model-View-Presenter (MVP) architecture. The simulation **Model** remains completely decoupled from Unity, containing no `UnityEngine` references or `Vector3` coordinate types.
*   **For Details:** See the dedicated guide on [unity-coordinate-spaces.md](file:///home/hp_prodesk/src/moonfall/docs/explanation/unity-coordinate-spaces.md).
