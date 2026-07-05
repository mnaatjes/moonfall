# Reference: Terrain Streaming Service Specification

The `TerrainStreamingService` handles level-of-detail (LOD) database loading and unloading. It monitors camera positions, traverses the lunar quadtree, and manages sector RAM allocations.

---

## 1. Interface and Methods

### Class: `TerrainStreamingService`

*   **`EvaluateLoadRequirements(GeoCoordinate cameraLookAt, float cameraAltitude) : void`**
    *   *Role:* Traverses the 6 quadtree roots, subdividing or merging nodes depending on camera distance and altitude.
*   **`LoadSectorData(LunarQuadtreeNode leafNode) : void`**
    *   *Role:* Reads heightmap and resource textures from storage, instantiates the `Sector` data model, and links it to the leaf node.
*   **`UnloadSectorData(LunarQuadtreeNode leafNode) : void`**
    *   *Role:* Discards grid cell arrays and sector references from memory when the camera leaves the zone, freeing up system RAM.

---

## 2. Class Diagram

```mermaid
classDiagram
    class TerrainStreamingService {
        +EvaluateLoadRequirements(GeoCoordinate cameraLookAt, float cameraAltitude) void
        -LoadSectorData(LunarQuadtreeNode leafNode) void
        -UnloadSectorData(LunarQuadtreeNode leafNode) void
    }
    class LunarQuadtreeNode {
        +int Depth
        +GeoCoordinate Center
        +Sector SectorData
        +LunarQuadtreeNode[] Children
    }
    class Sector {
        +string SectorId
        +bool IsLoaded
    }
    TerrainStreamingService --> LunarQuadtreeNode : Traverses and Modifies
    LunarQuadtreeNode --> Sector : Holds
```

---

## 3. Streaming Sequence

This sequence outlines the check and load execution flow during the game update loop:

```mermaid
sequenceDiagram
    participant Loop as Unity Update Loop
    participant TSS as TerrainStreamingService
    participant QTNode as LunarQuadtreeNode
    participant Disk as Storage Disk

    Loop->>TSS: "EvaluateLoadRequirements(camLookAt, camAltitude)"
    TSS->>QTNode: "Distance Check (Camera to Node Center)"
    opt When Node needs subdivision (Zoomed In)
        TSS->>QTNode: "Subdivide into four children"
    end
    opt When Node is Leaf and within range
        TSS->>TSS: "LoadSectorData(Node)"
        TSS->>Disk: "Async Read Heightmap and Resource Maps"
        Disk-->>TSS: "Return raw float arrays"
        TSS->>QTNode: "Instantiate and link SectorData"
    end
    opt When Node is out of range
        TSS->>TSS: "UnloadSectorData(Node)"
        TSS->>QTNode: "Clear SectorData reference (Garbage Collect)"
    end
```
