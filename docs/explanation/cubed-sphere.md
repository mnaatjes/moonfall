# Explanation: Cube Sphere Terminology and Projections

This document clarifies the concepts, mathematical variations, and terms associated with projecting flat square grids onto a spherical surface for the simulation.

---

## 1. Core Terminology

### A. Cube Sphere (Cubed Sphere)
*   **Definition:** The general, catch-all term for any sphere constructed by taking the six flat faces of a cube and projecting them outward onto a spherical surface.
*   **Context:** A broad category that includes various mathematical projection methods (such as simple vector normalization or equiangular mapping).

### B. Quadrilateralized Spherical Cube (QSC)
*   **Definition:** A highly specific, mathematically rigorous version of a Cube Sphere projection. It was developed by NASA scientists in the 1970s for cosmic microwave background mapping.
*   **Context:** It is an **equal-area** projection. The mathematical mapping equations are specifically tuned so that every grid cell on the sphere represents the exact same physical surface area, which is vital for accurate geospatial data calculations.

### C. Quad Sphere
*   **Definition:** A term primarily used in 3D modeling and computer graphics (such as in Blender or ZBrush).
*   **Context:** It describes any spherical 3D mesh made entirely of quadrilateral (four-sided) polygons, as opposed to a UV sphere (which has triangles converging at the poles) or an icosphere (made entirely of triangles). While all Cube Spheres are Quad Spheres, "Quad Sphere" is the artist/modeling term rather than a scientific mapping system.

---

## 2. Geometric Concepts

### A. Projection
*   **Definition:** The mathematical translation of coordinates from one geometric space or surface type onto another.
*   **Application:** Converting a flat 2D point $(x, y)$ on a cube's face into a 3D coordinate $(X, Y, Z)$ on the curved surface of a sphere.

### B. Side vs. Face
*   **Face:** A 2D flat polygon that bounds a 3D object. In the context of a CubeSphere, it refers to one of the 6 major grid regions (Front, Back, Left, Right, Top, Bottom).
*   **Side (or Edge):** A 1D line segment that connects two vertices on a shape. In a CubeSphere, it refers to the boundaries where adjacent faces meet. Gaps or mismatches along these sides must be prevented so the global terrain mesh remains continuous.

### C. Stretch Intensity and Distortion Terms
*   **Scale Factor / Stretch Factor ($s$):** The direct multiplier used to extend the length of a vector from its position on the cube face to the sphere surface.
*   **Grid Distortion (or Grid Inflation):** The general term for how the uniform square grid cells on the flat cube face become warped and unevenly sized when projected onto the sphere.
*   **Metric Tensor / Jacobian:** The mathematical tools used in differential geometry to measure the exact rate of "stretch intensity" (deformation of distance, area, and angles) at any given coordinate on the surface.
*   **Area Deformation (or Area Scale):** The ratio of the area of a grid cell on the sphere to its original area on the cube, measuring how much a cell "inflates".

---

## 3. Circumference and Cube Inscription

To visualize how the flat cube becomes a round sphere, let's look at the mathematical mechanics of placing a cube inside a sphere:

### A. The Starting State (The Inscribed Cube)
*   Imagine a 3D cube placed inside a sphere.
*   The 8 corners of the cube are touching the sphere's inner wall.
*   The centers of the 6 flat faces do not touch the sphere; they sit closer to the center of the sphere.

### B. The Math of Inscription (Unit Example)
*   Suppose we have a cube with corners at coordinates $(\pm 1, \pm 1, \pm 1)$.
*   **Distance to Corners:** The distance from the center $(0,0,0)$ to any corner is $\sqrt{1^2 + 1^2 + 1^2} = \sqrt{3} \approx 1.732\text{ units}$.
*   **Distance to Face Centers:** The distance from the center to the middle of any face is exactly $1.0\text{ unit}$.
*   If this cube is placed inside a sphere of radius $\sqrt{3}$, only the corners touch the sphere.

### C. Purpose of Finding "Distance to Face Centers"
Understanding the distance from the center to the face centers serves a key purpose in the projection math:
*   **Measuring Topographical Variance:** It highlights that the cube's flat surface varies in distance from the center (from $1.0$ at the face center to $1.732$ at the corners).
*   **Calculating the Stretch Factor:** To inflate the cube into a sphere, the projection formula must calculate how much to stretch each point. The face centers (at distance $1.0$) must be pushed outward by a larger ratio than the corners (at distance $1.732$) to achieve a uniform sphere radius.

### D. How Projection "Stretches" the Cube
To turn the flat cube into a perfect sphere, we must project all points on the cube surface outward:
*   We draw a straight line (a vector) from the center of the sphere, through a point on the flat cube face, and extend it until it hits the sphere's surface.
*   This normalizes the distance of every point to be exactly equal to the sphere's radius ($R$).
*   The centers of the faces (originally at distance $1.0$) are pushed outward by the greatest amount, while the corners (already at the sphere's boundary) are not pushed outward at all. This "inflates" the flat faces into curved surfaces.
