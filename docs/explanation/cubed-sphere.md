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

### D. Position Vector
*   **Definition:** In computer graphics and coordinate geometry, vectors are used to represent static coordinate positions relative to an origin point.
*   **Mechanics:** When representing a point $(x, y, z)$ as a position vector $\vec{p}$, we are describing a line that starts at the origin $(0,0,0)$ and points directly to $(x,y,z)$.
    *   **Direction:** The straight line pointing from $(0,0,0)$ to $(x,y,z)$.
    *   **Magnitude:** The physical distance from $(0,0,0)$ to $(x,y,z)$, calculated using the 3D distance formula.

A vector does contain both direction and magnitude, but we write them as separate mathematical variables when we need to extract and use them individually:

*   **The Vector $\vec{p} = (x, y, z)$:** This single variable encodes both the direction and magnitude simultaneously inside the three numbers.
*   **The Magnitude $\|\vec{p}\|$:** This is a function we apply to the vector to extract only the length/distance as a single number (e.g., $1.732$).
*   **The Direction (Unit Vector $\hat{p}$):** This is a function we apply to extract only the direction pointing towards that position, stripping away the distance. It is calculated as:
$$\hat{p} = \frac{\vec{p}}{\|\vec{p}\|}$$

### Analogy: A GPS Instruction
Think of the vector $\vec{p}$ as a complete instruction: *"Walk Northeast for 10 miles."*
*   The vector contains the whole instruction.
*   If we ask for the magnitude, we extract just the distance: *"10 miles"*.
*   If we ask for the direction, we extract just the heading: *"Northeast"*.

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
*   **Vector Scaling:** We draw a straight line (a vector) from the center of the sphere, through a point on the flat cube face, and extend it until it hits the sphere's surface.
*   **Distance Normalization:** This normalizes the distance of every point to be exactly equal to the sphere's radius ($R$).
*   **Uneven Expansion:** The centers of the faces (originally at distance $1.0$) are pushed outward by the greatest amount, while the corners (already at the sphere's boundary) are not pushed outward at all. This "inflates" the flat faces into curved surfaces.

---

## 4. Mathematical Representations and Equations

This section defines the mathematical symbols and equations used to project a flat cube coordinate onto a sphere of radius $R$.

### A. Variable Reference
*   **$\vec{p} = (x, y, z)$:** The 3D position vector of a point on the flat surface of the cube.
*   **$d = \|\vec{p}\| = \sqrt{x^2 + y^2 + z^2}$:** The Euclidean distance from the center $(0,0,0)$ to the point $\vec{p}$.
*   **$d_{\text{face}}$:** The Distance-to-Face-Centers ($d_{\text{face}} = 1.0\text{ unit}$ on a unit cube).
*   **$d_{\text{corner}}$:** The Distance-to-Corners ($d_{\text{corner}} = \sqrt{3} \approx 1.732\text{ units}$ on a unit cube).
*   **$R$:** The physical radius of the target sphere (e.g., $1737.4\text{ km}$ for the Moon).
*   **$s$:** The local Scale Factor (stretch factor) calculated for point $\vec{p}$.
*   **$\vec{p}_{\text{sphere}}$:** The final 3D coordinate vector of the projected point on the sphere surface.

### B. Equations

#### 1. Deriving the Scale Factor ($s$)
The scale factor for any point is the ratio of the target sphere radius to the point's current distance from the center:
$$s = \frac{R}{d} = \frac{R}{\|\vec{p}\|}$$

#### 2. Projecting to the Sphere Surface ($\vec{p}_{\text{sphere}}$)
Multiplying the original flat coordinate vector by the scale factor yields its position on the sphere:
$$\vec{p}_{\text{sphere}} = s \cdot \vec{p} = \left(\frac{R}{\|\vec{p}\|}\right) \cdot \vec{p}$$

#### 3. Range of Stretch
*   **Maximum Scale (At Face Center):** $s_{\text{max}} = \frac{R}{d_{\text{face}}} = R$
*   **Minimum Scale (At Corner):** $s_{\text{min}} = \frac{R}{d_{\text{corner}}} = \frac{R}{\sqrt{3}} \approx 0.577 \cdot R$

---

## Appendix: Mathematical Writing Conventions (Notation)

Mathematicians and programmers use distinct symbols to indicate the geometric nature of a variable:

| Value Type | Math Symbol | Example | Explanation |
| :--- | :--- | :--- | :--- |
| **Vector** | Arrow overhead OR Bold font | $\vec{v}$ or $\mathbf{v}$ | Indicates a full package of direction and size. |
| **Magnitude** | Double/single vertical bars | $\|\vec{v}\|$ or $|\vec{v}|$ | Indicates the extracted scalar size/length of that vector. |
| **Scalar** | Standard lowercase/italicized letter | $s$ or $T$ | Indicates a plain, single number (no bold, no arrow). |
| **Distance** | Standard scalar letter | $d$ or $r$ | A specific scalar representing length or spatial offset. |

### Scalar and Velocity Vector Example

To clarify the difference between vectors, magnitudes, and scalars outside of spatial distances, consider a storm:

*   **The Scalar:** The raw number `50` (or the speed `50 mph`).
*   **The Definition:** A scalar is simply a single number that represents a size or quantity, with no direction attached.
*   **The Relationship:**
    *   **Velocity Vector:** The full coordinate package: `("Northeast", 50)`.
    *   **Magnitude:** The size of that velocity vector (its speed).
    *   **Scalar:** The mathematical value of that magnitude (`50`).
*   **Other Scalars in the Storm:** The air temperature (`72`), the storm's pressure (`980`), and the duration of the storm (`3` hours) are also scalars. They are all single numbers with no directional vectors.
