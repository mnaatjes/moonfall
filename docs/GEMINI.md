# Documentation Guidelines: Diátaxis Framework

All documentation in this project MUST be structured according to the **Diátaxis** documentation framework. This ensures clarity, consistency, and usability for both developers and external collaborators.

## The Four Documentation Genres

Every document created under the `docs/` folder must belong to one of these four categories:

1.  **Tutorials (`docs/tutorials/`):**
    *   *Purpose:* Learning-oriented.
    *   *Goal:* Help a beginner complete a simple, guided walkthrough to get started. Do not explain *why* or offer multiple alternatives; provide a single, working path.
2.  **How-To Guides (`docs/how-to/`):**
    *   *Purpose:* Task-oriented.
    *   *Goal:* Guide an experienced user through the steps required to solve a specific, real-world problem (e.g., "How to connect a new UI button to the overlay controller").
3.  **Reference (`docs/reference/`):**
    *   *Purpose:* Information-oriented.
    *   *Goal:* Provide raw, objective facts, schemas, API parameters, and technical descriptions (e.g., Shader property lists or directory layouts).
4.  **Explanation (`docs/explanation/`):**
    *   *Purpose:* Understanding-oriented.
    *   *Goal:* Deepen background knowledge, architecture concepts, design decisions, and reasons *why* certain patterns or tools were chosen (e.g., why texture packing was preferred).

## Directory Structure

Ensure documentation files are saved in the correct sub-directory:
```text
docs/
├── GEMINI.md            # This directive file
├── tutorials/           # Learning walkthroughs
├── how-to/              # Step-by-step guides for tasks
├── reference/           # API specs, configuration listings
└── explanation/         # Architecture, design decisions, and concept notes
```
