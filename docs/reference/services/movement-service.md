# Reference: Entity Movement Service Specification

The `EntityMovementService` handles physics step updates for movable entities (Drones, Rovers) on the lunar surface, calculating speeds and performing sector boundary transfers.

---

## 1. Interface and Methods

### Class: `EntityMovementService`

*   **`MoveEntity(IMovableEntity entity, float deltaTime) : void`**
    *   *Role:* Steps the entity along its target path, adjusting its metric offsets.
*   **`CheckBoundaryCrossing(IMovableEntity entity) : void`**
    *   *Role:* Evaluates whether the entity's new metric offsets have exceeded its current Sector boundary limits.

---

## 2. Class Diagram

```mermaid
classDiagram
    class EntityMovementService {
        +MoveEntity(IMovableEntity entity, float deltaTime) void
        -CheckBoundaryCrossing(IMovableEntity entity) void
    }
    class IMovableEntity {
        <<interface>>
        +string SectorId
        +float LocalX
        +float LocalZ
        +float Speed
        +UpdatePosition(float newX, float newZ) void
    }
    class CoordinateService {
        +LocalToGeodetic(Sector, LocalOffset) GeoCoordinate
        +GeodeticToLocal(Sector, GeoCoordinate) LocalOffset
    }
    EntityMovementService ..> IMovableEntity : Updates
    EntityMovementService --> CoordinateService : Uses for conversions
```

---

## 3. Movement and Boundary Crossing Sequence

Detailed sequence showing how movement ticks are processed and sector boundaries resolved:

```mermaid
sequenceDiagram
    participant EMS as EntityMovementService
    participant EM as IMovableEntity (Model)
    participant CS as CoordinateService

    EMS->>EM: Query Position & Speed
    EM-->>EMS: return "Sector_A", Offset, Speed
    EMS->>EMS: Calculate Next local position
    EMS->>EMS: Detect LocalOffset outside boundary bounds
    EMS->>CS: LocalToGeodetic(Sector_A, nextOffset)
    CS-->>EMS: return GeoCoordinate
    EMS->>EMS: Identify destination Sector_B
    EMS->>CS: GeodeticToLocal(Sector_B, GeoCoordinate)
    CS-->>EMS: return newLocalOffset
    EMS->>EM: UpdatePosition(newLocalOffset)
    EMS->>EM: Set SectorId = "Sector_B"
```
