# Explanation: Unity Coordinate Spaces and Rendering

This document explains how the Unity engine handles virtual 3D volumes, coordinates, frames of reference, and cameras in the context of the simulation.

---

## 1. Frame of Reference and World Origin

Unity utilizes a Cartesian coordinate system to define a 3D virtual volume.

### A. The Left-Handed Coordinate System
Unity uses a **left-handed coordinate system** to define directions in 3D space:
*   **$+X$ (Red Axis):** Points to the **Right**.
*   **$+Y$ (Green Axis):** Points **Up**.
*   **$+Z$ (Blue Axis):** Points **Forward** (depth, away from the viewer).

### B. The World Origin
*   **Definition:** The absolute coordinate **$(0, 0, 0)$** in Unity's global virtual space.
*   **Role:** Serves as the fixed, permanent anchor point from which all object positions in the scene are measured. It does not move when the game camera moves.

---

## 2. Coordinate Spaces

Unity separates coordinates into different coordinate systems depending on whether they represent positions in the virtual 3D world or on the player's 2D screen:

### A. World Space (3D)
*   **Definition:** The coordinate system of the entire 3D virtual environment.
*   **Measurement:** Units are mapped by convention to physical meters ($1\text{ unit} = 1\text{ meter}$).
*   **Origin:** Fixed at the global World Origin $(0, 0, 0)$.

### B. Screen Space (2D)
*   **Definition:** The coordinate system mapping directly to the pixels of the player's physical monitor.
*   **Measurement:** Units are represented in screen pixels (e.g., $1920 \times 1080$).
*   **Origin:** The bottom-left corner of the screen/window is $(0, 0)$. The top-right corner is $(\text{screen width}, \text{screen height})$.

---

## 3. The Camera Projection

The **Camera** acts as the translator between coordinate spaces:

*   **Projection:** The camera reads the 3D coordinates of objects in **World Space** and projects them onto the flat 2D plane of **Screen Space** based on its position, rotation, and field of view.
*   **Viewport Space:** A normalized coordinate system where $(0,0)$ is the bottom-left of the camera's view, and $(1,1)$ is the top-right (independent of pixel resolution). The camera uses this normalized space to map world coordinates to screen pixels.

---

## 4. Understanding Unity "Scenes"

To clarify how the virtual volume relates to what is rendered on screen, it is necessary to define the concept of a **Scene**:

### A. What is a "Scene"?
*   **Definition:** A Scene is a virtual environment container (like a level, map, or workspace file).
*   **Contents:** It contains all the 3D meshes (the Moon globe), cameras, lighting, and script engines that make up that part of the game.

### B. Is it what is drawn on the screen?
*   **No:** The screen only shows what the Camera is currently looking at.
*   **Example:** If the Moon sphere is in the Scene, but the camera is rotated away facing empty space, the screen will show black space. However, the Moon still exists in the active Scene.

### C. Is the Scene equivalent to Unity's Global Virtual Space?
*   **Yes:** While a scene is active, it defines the active Global Virtual Space. The origin $(0,0,0)$ is the center of this active scene container.
*   **Note:** You can think of a Scene as a virtual room. The World Origin is the exact center of that room, and the Camera is a person walking around the room holding a video camera. What is shown on the screen is only what the camera person points at, not the entire room.

---

## 5. View Optimization and Culling Systems

To maintain game performance, Unity and the developers use several culling systems to manage which assets in the active Scene are rendered at any given time:

### A. Frustum Culling
*   **Definition:** The automatic process of discarding rendering calculations for objects that lie outside the camera's field of view (the view frustum).
*   **Developer Participation:** **Automatic.** This is handled inherently by Unity's rendering pipeline; no developer configuration is required.

### B. Level of Detail (LOD) and LOD Group Components
*   **Definition:** Swapping complex 3D models with simplified, lower-polygon versions (or hiding them entirely) as the camera moves further away.
*   **Developer Participation:** **Explicitly Configured.** Developers must attach and configure `LOD Group` components on game assets to define the distance thresholds and models used at varying camera distances.

### C. Occlusion Culling
*   **Definition:** Hides objects that are technically inside the camera's view frustum but are completely blocked from view by other solid geometry (such as terrain or buildings).
*   **Developer Participation:** **Explicitly Configured.** Developers must designate static geometry as occluders and bake occlusion data maps within the Unity Editor.
