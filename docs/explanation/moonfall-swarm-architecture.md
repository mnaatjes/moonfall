# Explanation: Moonfall Drone Swarm Architecture

This document describes the architectural specifications, user-flows, and class relationships for the Moonfall drone swarm deployment and sector-claiming mechanics.

---

## 1. System Overview

The Moonfall Drone Swarm represents a late-game, high-fidelity mapping asset. Instead of operating as a single localized probe or orbiter, it operates as a coordinated swarm of 4 mobile units. 

Its primary gameplay objective is to allow the player to **claim a land sector** for construction by conducting a definitive high-resolution survey of the sector's corners.

---

## 2. Model Structure (Class Diagram)

This class diagram details the properties and methods used to orchestrate the swarm descent and coordinate mapping:

```mermaid
classDiagram
    class SpacecraftModel {
        +string Id
        +string Name
        +SpacecraftState CurrentState
    }

    class DroneSwarmModel {
        +Vector2[] DronePositions
        +SectorModel TargetedSector
        +List~InstrumentModel~ SwarmInstruments
        +DeployToSector(SectorModel sector)
        +UpdateDescent(float dt)
    }

    class SectorModel {
        +Vector2 BoundaryMin
        +Vector2 BoundaryMax
        +bool IsClaimed
        +float ScanFidelity
        +ClaimSector()
    }

    class ResourceGridModel {
        +float[,] ResourceDensities
        +float[,] RevealedFidelity
        +UpdateFidelityGrid(SectorModel sector)
    }

    SpacecraftModel <|-- DroneSwarmModel : Inherits
    DroneSwarmModel --> SectorModel : Focuses & Descends
    SectorModel --> ResourceGridModel : Updates Resolution
```

---

## 3. User-Flow and Deployment Sequence

The sequence below tracks the lifecycle of the swarm from ordering, through transit and orbit, to localized landing and sector claiming:

```mermaid
sequenceDiagram
    actor Player
    participant MM as MissionManagerModel
    participant DS as DroneSwarmModel
    participant SEC as SectorModel
    participant GRID as ResourceGridModel

    %% Phase 1: Purchase and Launch
    Player->>MM: Order Moonfall Swarm Launch (Payload)
    Note over MM: Calculates Transit Time (Transit State)
    MM-->>Player: Notify: "Moonfall Swarm inserted into lunar orbit"

    %% Phase 2: Selection & Target Landing
    Player->>DS: DeployToSector(Sector_Alpha)
    Note over DS: Swarm shifts state to Descending.<br/>Calculates corner target coordinates.

    %% Phase 3: Descent and Planting
    DS->>SEC: Land in 4 Corners (altitude = 0)
    Note over DS: Units deploy ground anchors

    %% Phase 4: Resolution Mapping & Claim
    SEC->>GRID: UpdateFidelityGrid(Sector_Alpha)
    Note over GRID: Sets localized ScanFidelity = 1.0 (Maximum resolution)
    SEC->>SEC: ClaimSector()
    SEC-->>Player: Notify: "Sector Alpha claimed. Base construction unlocked."
```

---

## 4. Architectural Rules for Swarm Implementation

To ensure strict separation of concerns following our MVP guidelines:
1.  **Physics Decoupling:** The descent physics (ballistic path calculation to the corners) must be processed by the [OrbitalService](file:///home/hp_prodesk/src/moonfall/Assets/Scripts/Simulation/OrbitalService.cs) using formulas in [OrbitalMath.cs](file:///home/hp_prodesk/src/moonfall/Assets/Scripts/Simulation/OrbitalMath.cs).
2.  **State Constraints:** The `DroneSwarmModel` must not allow deployment to a sector that is already claimed or falls outside the active orbital path of the spacecraft.
3.  **UI Updates:** The transition from Low-Fidelity scanning to High-Fidelity scanning on the player's screen must be handled by the Presenter listening to the `OnMapUpdated` event fired by the `ResourceGridModel`.
