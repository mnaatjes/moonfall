# Reference: Lunar Surface Geodetic and Game Model Standards

This document establishes the real-world geodetic standards used by planetary agencies (NASA PDS) alongside the coordinate and terminology standards defined for the Moon Simulation game.

---

## 1. Real-World Geodetic Reference Data (NASA PDS)

To keep physical calculations realistic and ensure compatibility with planetary GIS datasets (such as LRO LOLA/LROC), the simulation adheres to the following planetary standards:

| Parameter | Value / Standard | Description |
| :--- | :--- | :--- |
| **Reference System** | Mean Earth/Polar Axis (ME) | The IAU-approved coordinate reference frame for lunar cartography. |
| **Coordinate Format** | Planetocentric Coordinates | Coordinates are computed from the Moon's center of mass. |
| **Latitude Range** | $-90.0^{\circ}$ to $+90.0^{\circ}$ | Decimal degrees. negative represents South, positive represents North. |
| **Longitude Range** | $0.0^{\circ}$ to $360.0^{\circ}$ | Decimal degrees. Measured continuously in the Eastern direction. |
| **Lunar Mean Radius** | $1737.4\text{ km}$ | The base spherical datum. Altitudes are expressed as offsets from this radius. |
| **Physical Units** | Metric SI System | Lengths are in meters/kilometers; mass in kilograms; time in seconds. |

---

## 2. In-Game Model Terminology

These defined terms map geographical GIS dataset boundaries to localized Unity game world coordinate spaces:

*   **Geodetic Coordinate (`GeoCoordinate`):** A location defined by `Latitude` (decimal degrees, $-90.0$ to $90.0$) and `Longitude` (decimal degrees, $0.0$ to $360.0$).
*   **Altitude / Elevation (`Elevation`):** The height offset in meters ($m$) relative to the base lunar radius sphere ($1737.4\text{ km}$).
*   **Sector (`Sector`):** A bounded geographical coordinate block (e.g., $1^{\circ} \times 1^{\circ}$ square) representing a building and exploration zone.
*   **Local Coordinates (`LocalOffset`):** Metrical offsets $(x, z)$ measured in meters from a sector's customized origin point, used to place structures inside Unity scenes.
*   **Tile / Cell (`GridCell`):** The atomic resolution unit of the terrain database, representing a single pixel on heightmap or resource textures.

---

## 3. Data Hierarchy & Streaming Logic

To simulate the entire Moon down to meter-scale construction details, the game utilizes a single **unified hierarchical database** representing three scales:

1.  **Macro Scale (The Globe):** Lower-resolution planet-wide heights and albedo mapping. Always kept in RAM for global camera views.
2.  **Medium Scale (Sectors):** Bounded geographical regions ($1.0^{\circ} \times 1.0^{\circ}$). Loaded as separate nodes.
3.  **Micro Scale (Grid Cells):** Metrical grid cells inside a Sector. Dynamically loaded/unloaded from disk on demand.

---

## 4. Quadtree Data Structure Models

The Moon is mathematically modeled as a cube with 6 faces, where each face is the root of a Quadtree structure. 

### A. The Quadtree Node Model: [LunarQuadtreeNode](file:///home/hp_prodesk/src/moonfall/Assets/Scripts/Simulation/LunarQuadtreeNode.cs)
*   **Role:** Represents a branch in the spatial directory. It is recursively subdivided as the player zooms in.
*   **Properties:**
    *   `int Depth` (0 = Face root, 5 = Individual Sector level)
    *   `GeoCoordinate Center`
    *   `GeoCoordinate BoundaryMin`, `GeoCoordinate BoundaryMax`
    *   `LunarQuadtreeNode[] Children` (4 sub-nodes, null if leaf node)
    *   `Sector SectorData` (Only initialized on leaf nodes when streamed in)

### B. The Sector Model: [Sector](file:///home/hp_prodesk/src/moonfall/Assets/Scripts/Simulation/Sector.cs)
*   **Role:** The playable territory container. It maps geographical boundaries to flat coordinates.
*   **Properties:**
    *   `string SectorId`
    *   `GeoCoordinate Origin` (South-West corner, maps to Unity local coordinates `Vector3.zero`)
    *   `GridCell[,] Grid` (2D grid of atomic tiles)
    *   `bool IsLoaded`

### C. The Grid Cell Model: [GridCell](file:///home/hp_prodesk/src/moonfall/Assets/Scripts/Simulation/GridCell.cs)
*   **Role:** Represents the micro-scale data cell.
*   **Properties:**
    *   `LocalOffset Offset` (meters from the Sector origin)
    *   `float Elevation` (height offset in meters)
    *   `float WaterIceDensity` (0.0 to 1.0)
    *   `float TitaniumDensity` (0.0 to 1.0)

---

## 5. On-Demand Streaming Service

The loading and unloading of high-fidelity data is managed by a dedicated C# business service.

### The Service: [TerrainStreamingService](file:///home/hp_prodesk/src/moonfall/Assets/Scripts/Simulation/TerrainStreamingService.cs)
*   **Role:** Reads camera coordinates and dynamically manages node allocations in the Quadtree.
*   **Methods:**
    *   `EvaluateLoadRequirements(GeoCoordinate cameraLookAt, float cameraAltitude)`: Recursively walks the quadtree. Determines which nodes require subdivision or merging based on distance.
    *   `LoadSectorData(LunarQuadtreeNode leafNode)`: Asynchronously reads heightmap (LOLA DEM) and resource segments from storage and instantiates the `Sector` data.
    *   `UnloadSectorData(LunarQuadtreeNode leafNode)`: Discards the `GridCell` array from memory to free up RAM when the camera moves away.
