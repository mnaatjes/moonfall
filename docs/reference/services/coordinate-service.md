# Reference: Coordinate Service Specification

The `CoordinateService` is a stateless utility service responsible for translating coordinates between the distinct scales of the simulation: local metrics, global geodetic angles, and virtual Unity coordinates.

---

## 1. Interface and Methods

### Class: `CoordinateService`

*   **`LocalToGeodetic(Sector sector, LocalOffset localPos) : GeoCoordinate`**
    *   *Role:* Translates a 2D metric offset from a specific sector's Southwest origin into a global Latitude/Longitude coordinate.
*   **`GeodeticToLocal(Sector sector, GeoCoordinate geoPos) : LocalOffset`**
    *   *Role:* Translates a global Latitude/Longitude coordinate into a 2D metric offset relative to the target sector's Southwest origin.
*   **`GeodeticToCartesian(GeoCoordinate geoPos, float elevation) : Vector3`**
    *   *Role:* Projects a spherical geodetic coordinate and altitude into 3D Cartesian coordinates. Note: This method is bridging the Model-View boundary and is primarily consumed by Presenters to set Unity positions.

---

## 2. Class Diagram

```mermaid
classDiagram
    class CoordinateService {
        +LocalToGeodetic(Sector sector, LocalOffset localPos) GeoCoordinate
        +GeodeticToLocal(Sector sector, GeoCoordinate geoPos) LocalOffset
        +GeodeticToCartesian(GeoCoordinate geoPos, float elevation) Vector3
    }
    class Sector {
        +string SectorId
        +GeoCoordinate Origin
    }
    class GeoCoordinate {
        +float Latitude
        +float Longitude
    }
    class LocalOffset {
        +float X
        +float Z
    }
    CoordinateService ..> Sector : Processes
    CoordinateService ..> GeoCoordinate : Returns/Converts
    CoordinateService ..> LocalOffset : Returns/Converts
```

---

## 3. Translation Sequence

Shows how the `EntityPresenter` utilizes the `CoordinateService` to position a newly spawned entity in Unity World Space:

```mermaid
sequenceDiagram
    participant P as EntityPresenter
    participant CS as CoordinateService
    participant M as EntityModel
    participant V as EntityView

    P->>M: Query Position (SectorId, LocalOffset)
    M-->>P: return "Sector_A", (10m, 50m)
    P->>CS: LocalToGeodetic(Sector_A, (10m, 50m))
    CS-->>P: return GeoCoordinate
    P->>CS: GeodeticToCartesian(GeoCoordinate, elevation)
    CS-->>P: return Vector3
    P->>V: Set GameObject position (Vector3)
```
