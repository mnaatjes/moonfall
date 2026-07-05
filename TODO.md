# Moon Simulation Project: Setup and Implementation Plan

This checklist outlines the steps to build a Moon simulation game in Unity on Windows 11, using raw Lunar GIS data and interfacing with the Antigravity agent in WSL2.

---

## Phase 0: Required Tooling (Windows 11)
- [ ] **Unity Hub & Unity Editor:** Recommended version: **Unity 6 (LTS)**.
- [ ] **Visual Studio Code (or Rider):** Primary IDE for writing C# code.
- [ ] **QGIS:** Free desktop GIS software for processing planetary data.
- [ ] **Git:** For version control.

---

## Phase 1: Installation & Directory Configuration

### 1. Install Windows 11 Tools
*   Download and install [Unity Hub](https://unity.com/download). From the Hub, install the latest LTS version of the editor (e.g., Unity 6). Add the **WebGL Build Support** and **Windows Build Support** modules.
*   Download and install [VS Code](https://code.visualstudio.com/). Install the **Extension Pack for Unity** and the **Remote - SSH** extension.
*   Download and install [QGIS](https://qgis.org/).

### 2. Set Up the Shared Directory (Ubuntu Server & Windows 11)
*   **Host Files on the Ubuntu Server:**
    *   The project directory is hosted on the Ubuntu Server: `hp_prodesk@192.168.1.146` (Port: `2222`).
    *   Directory path: `/home/hp_prodesk/src/moonfall`.
*   **Mount the Server Directory in Windows 11:**
    *   **Option A (Samba / SMB - Recommended for Unity Hub performance):** Set up a Samba share on the Ubuntu server for `/home/hp_prodesk/src/moonfall` and map it as a network drive (e.g., `Z:\`) in Windows Explorer.
    *   **Option B (SSHFS-Win):** Use SSHFS-Win on Windows to mount the remote SSH directory over port 2222:
        ```cmd
        net use Z: \\sshfs.r\hp_prodesk@192.168.1.146!2222\home\hp_prodesk\src\moonfall
        ```
*   **Open Project in Unity Hub:**
    *   In Unity Hub, click **Add project from disk**.
    *   Select the mapped network drive (e.g., `Z:\`) representing the server path.
*   **Agent Execution:** The Antigravity agent runs inside the Ubuntu server environment at `/home/hp_prodesk/src/moonfall`, allowing real-time monitoring and file edits.

---

## Phase 2: Data Acquisition (Lunar GIS)
- [ ] **Topography & Elevation (DEM):** Download the LOLA (Lunar Orbiter Laser Altimeter) DEM heightmaps from NASA's PDS (Planetary Data System) or USGS Astrogeology Science Center.
- [ ] **Albedo / Surface Imagery:** Fetch LRO (Lunar Reconnaissance Orbiter) Wide Angle Camera (WAC) albedo mosaics.
- [ ] **Resource & Radiation Data:** Obtain hydrogen abundance maps (neutron spectrometer data from Lunar Prospector/LRO) and radiation models (CRaTER instrument data).

---

## Phase 3: Data Processing & Optimization
- [ ] **Process in QGIS:** Import the raw Lunar files into QGIS on Windows. Crop the data to your desired coordinates.
- [ ] **Implement Texture Packing (Optimization):** Export and pack the raw datasets into the color channels of a single optimized texture file:
    *   **Red Channel:** Radiation levels.
    *   **Green Channel:** Hydrogen/Water ice resources.
    *   **Blue Channel:** Albedo/Reflectivity values.
- [ ] **Import to Unity:** Place the heightmaps and packed texture inside the `Assets/Textures/` folder.

---

## Phase 4: Material & Planet Shader Architecture (Unity)
- [ ] **Set up CubeSphere/Quadtree:** Implement or import a LOD system (such as ArcGIS SDK or a custom CubeSphere mesh) to handle the Lunar sphere geometry.
- [ ] **Create the Master Shader:** Build a Unity Shader Graph (or HLSL shader) that accepts multiple texture inputs simultaneously:
    *   `BaseTexture` (Albedo/Visual map)
    *   `HeightMap` (LOLA Topography for vertex displacement)
    *   `OverlayDataMap` (The packed texture containing Radiation, Resources, and Albedo channels)
- [ ] **Expose Shader Properties:** Expose floating-point parameters (opacities) to control how much each overlay is blended onto the base globe (e.g., `_RadiationOpacity` and `_ResourceOpacity` from `0.0` to `1.0`).
- [ ] **Implement Color Ramp Blending:** In the shader, use a Gradient (Color Ramp) to convert the grayscale radiation values into an intuitive heatmap (e.g., green for low, red for high) before blending it onto the physical topography.

---

## Phase 5: Camera Controls, UI, & C# logic
- [ ] **Orbit & Pan Camera Script:** Write `CameraController.cs` to allow 360-degree rotation and panning around the Moon's coordinates.
- [ ] **Zoom and Projection Morph Script:** Implement the transition logic converting spherical coordinates to flat map views at high zoom levels.
- [ ] **Build the Overlay Manager (`OverlayController.cs`):** Attach a C# script to the Moon GameObject to interface with your UI. 
- [ ] **Implement Dynamic Material Updates:** Set up the C# script to update the material's properties at runtime when the player toggles a UI overlay button using:
    ```csharp
    renderer.material.SetFloat("_RadiationOpacity", targetValue);
    ```
- [ ] **Data Read Controller:** Implement a script that queries the underlaying resource/radiation data texture at the player's cursor coordinates.
