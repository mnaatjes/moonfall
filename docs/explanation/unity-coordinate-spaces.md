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
