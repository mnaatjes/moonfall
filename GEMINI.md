# Project State Hand-Off: Moonfall Simulation

This document summarizes the current design state, recent accomplishments, and active tasks for the next development session.

---

## 1. Project Context & Current Architecture
The project is a Moon simulation game in Unity 6, utilizing raw NASA planetary geodetic data. We enforce a strict **Model-View-Presenter (MVP)** architectural pattern to keep all simulation and orbital physics decoupled from Unity:

*   **Models:** Pure C# classes (no `UnityEngine` dependencies).
*   **Views:** Dumb `MonoBehaviour` components handling rendering, UI, and raw input forwarding.
*   **Presenters:** Mediators connecting Models and Views.
*   **Design Framework:** Follows Diátaxis documentation guidelines under `docs/`.

---

## 2. Directory Layout of Architectural Work

*   [mvp-pattern-architecture.md](file:///home/hp_prodesk/src/moonfall/docs/explanation/mvp-pattern-architecture.md) – The core MVP boundaries and hard coding rules, using a Lunar Lander simulation as a code reference.
*   [models.md](file:///home/hp_prodesk/src/moonfall/docs/explanation/models.md) – UML class layout and registrations for core entities (`LaunchVehicleModel`, `SpacecraftModel`, `InstrumentModel`, `ResourceGridModel`, `MissionManagerModel`, `DroneSwarmModel`, `SectorModel`).
*   [orbital-simulation-architecture.md](file:///home/hp_prodesk/src/moonfall/docs/explanation/orbital-simulation-architecture.md) – Unified interface strategy (`IOrbitingEntity`) and service architecture (`OrbitalService`, `OrbitalManager`) for stepping celestial bodies and spacecraft orbits using Keplerian physics.
*   [moonfall-swarm-architecture.md](file:///home/hp_prodesk/src/moonfall/docs/explanation/moonfall-swarm-architecture.md) – Deployment sequence and class associations for the 2028 Moonfall propulsive hopping drone swarm and sector-claiming mechanics.
*   [lunar-surface.md](file:///home/hp_prodesk/src/moonfall/docs/reference/lunar-surface.md) – Real-world NASA PDS geodetic reference standards mapped to our in-game spatial hierarchy (Quadtree leaf nodes, `Sector` loading, and the `TerrainStreamingService`).

---

## 3. Databases Created (`docs/reference/`)

We have separated quantitative data sheets from qualitative narrative profiles using a 1:1 mirrored structure:

| Category | Specification Sheets (Numbers / Math) | Narrative Profiles (Lore / History) |
| :--- | :--- | :--- |
| **Launch Vehicles** | [specifications/launch-vehicles.md](file:///home/hp_prodesk/src/moonfall/docs/reference/specifications/launch-vehicles.md) | [profiles/launch-vehicles.md](file:///home/hp_prodesk/src/moonfall/docs/reference/profiles/launch-vehicles.md) |
| **Spacecraft** | [specifications/spacecraft.md](file:///home/hp_prodesk/src/moonfall/docs/reference/specifications/spacecraft.md) | [profiles/spacecraft.md](file:///home/hp_prodesk/src/moonfall/docs/reference/profiles/spacecraft.md) |
| **Instruments** | [specifications/instruments.md](file:///home/hp_prodesk/src/moonfall/docs/reference/specifications/instruments.md) | [profiles/instruments.md](file:///home/hp_prodesk/src/moonfall/docs/reference/profiles/instruments.md) |
| **Resources** | [specifications/resources.md](file:///home/hp_prodesk/src/moonfall/docs/reference/specifications/resources.md) | [profiles/resources.md](file:///home/hp_prodesk/src/moonfall/docs/reference/profiles/resources.md) |

---

## 4. Next Session Priorities

1.  **Drafting C# Models:** Write the pure C# classes for the models detailed in `models.md`, beginning with [LaunchVehicleModel.cs](file:///home/hp_prodesk/src/moonfall/Assets/Scripts/Simulation/LaunchVehicleModel.cs).
2.  **Implementing Physics Logic:** Program the stateless equations inside [OrbitalMath.cs](file:///home/hp_prodesk/src/moonfall/Assets/Scripts/Simulation/OrbitalMath.cs) to process Keplerian orbit periods and localized ballistic trajectory hops.
3.  **Writing Services:** Implement the [OrbitalService.cs](file:///home/hp_prodesk/src/moonfall/Assets/Scripts/Simulation/OrbitalService.cs) and [TerrainStreamingService.cs](file:///home/hp_prodesk/src/moonfall/Assets/Scripts/Simulation/TerrainStreamingService.cs) to process the runtime simulation loops.
