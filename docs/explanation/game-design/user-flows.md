# Instructions: User Flows and Navigation

Use this document to map out the player's UX journey, screen transitions, and input mappings. It should cover:

1.  **Camera Transition Flow:**
    *   *Step 1:* Global Orbit Sphere (360-degree rotation, zooming).
    *   *Step 2:* Sector Selection (Double-clicking a quadtree leaf node quadrangle).
    *   *Step 3:* Surface Morphing (Camera zooms and orthogonalizes to display the flat local metric grid).
2.  **Building Construction Flow:**
    *   *Step 1:* Player clicks build menu button.
    *   *Step 2:* Cursor shows building hologram snapped to local metric cells.
    *   *Step 3:* Player left-clicks to place; Model verifies resource subtraction.
3.  **Drone Mission Flow:**
    *   *Step 1:* Player selects drone, click "Deploy".
    *   *Step 2:* Player selects target sector; Service calculates pathway.
    *   *Step 3:* Drone enters flight loop; Presenter updates Unity position.
