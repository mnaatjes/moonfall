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
