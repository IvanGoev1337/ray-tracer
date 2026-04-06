# Ray Tracer

A small educational C++ project for experimenting with geometric ray tracing, custom vector math, and optical surface interactions.

## Overview

This repository contains a Visual Studio C++ project that implements the core pieces of a simple ray-tracing pipeline from scratch:

- custom 2D and 3D vector classes;
- ray representation and geometric operations;
- intersections with planes, cones, and axis-symmetric spherical surfaces;
- reflection and refraction logic;
- propagation of a bundle of rays through a small optical system;
- export of traced ray coordinates to a text file.

The current demo configuration builds a 12×12 ray grid, traces it through an axis-symmetric spherical surface, projects the result onto an image plane, and writes the resulting point coordinates to `D.txt`.

## Features

- Custom `vector2` and `vector` math implementation
- `ray` abstraction with direction, position, and activity state
- Support for geometric primitives such as:
  - plane
  - cone
  - axis-symmetric spherical surface
- Reflection from optical surfaces
- Refraction between media with different refractive indices
- Handling of total internal reflection / invalid ray states
- Ray bundle (`emiter`) propagation through the system
- Basic multithreading for grouped ray processing
- Text export of traced results for further analysis

## Project Structure

```text
ray-tracer/
├── TopologicOptimization/
│   ├── TopologicOptimization.cpp      # entry point and demo setup
│   ├── rayTrace.cpp                   # optical system container / tracing flow
│   ├── ray.cpp                        # rays, surfaces, emitter logic
│   ├── vector.cpp                     # vector math classes
│   ├── A.txt
│   ├── B.txt
│   ├── C.txt
│   ├── D.txt                          # output file with traced point coordinates
│   └── TopologicOptimization.vcxproj  # Visual Studio project
├── x64/
└── README.md
```

## How the Demo Works

In its current form, the program:

1. Creates a rectangular grid of input rays at the entrance plane.
2. Launches rays along the positive Z direction.
3. Configures one axis-symmetric spherical surface.
4. Sets refractive indices for two media.
5. Traces the ray bundle through the surface.
6. Transfers the resulting rays to the image plane.
7. Saves the output coordinates to `D.txt`.

This makes the project useful as a compact sandbox for studying how a ray bundle changes after interacting with a surface.

## Build and Run

### Requirements

- Windows
- Visual Studio with C++ workload
- MSVC toolset compatible with `v143`

### Build steps

1. Open `TopologicOptimization/TopologicOptimization.vcxproj` in Visual Studio.
2. Select a build configuration such as `Debug x64`.
3. Build the project.
4. Run the executable.

After execution, inspect `D.txt` to see the resulting traced coordinates.

## Configuration Notes

The main parameters are currently hardcoded in `TopologicOptimization.cpp`.

Useful values to modify:

- `numberRay` — controls the density of the ray grid
- `mirror.setSurface(R, d, 0)` — surface radius and placement
- `mirror.n[0]` and `mirror.n[1]` — refractive indices of the two media
- image plane parameters

## Implementation Notes

This is an educational / experimental codebase rather than a production renderer.

A few practical notes:

- The project uses direct inclusion of `.cpp` files instead of header/source separation.
- Some values and workflow steps are hardcoded for a specific experiment.
- Logging and comments are mixed, including Russian-language debug output.
- The code focuses on ray geometry and optical interaction, not on image synthesis, shading, or GPU rendering.

## Possible Improvements

- Split implementation into `.h` and `.cpp` files
- Add scene/config input from external files
- Export results in CSV format
- Add visualization of ray paths
- Support multiple surfaces and full optical trains
- Add unit tests for vector math and intersection logic
- Replace ad-hoc threading with a cleaner parallel processing design

## Purpose

This project is best viewed as a study project for:

- computational geometry;
- introductory optical simulation;
- ray-surface intersection logic;
- refraction and reflection algorithms;
- experimenting with custom C++ math and tracing structures.

## License

No license is specified in the repository. Add one if you want other people to reuse the code with clear terms.
