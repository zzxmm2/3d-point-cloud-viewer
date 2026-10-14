# 3D Point Cloud Viewer & Surface Reconstruction Engine

An interactive, high-performance 3D point cloud visualizer and surface reconstruction testbed built with Modern C++ (C++20), OpenGL 4.1 Core Profile, and Dear ImGui.

---

## 1. Project Motivation & Portfolio Goals

- **3D Software Engineering**: Showcase expertise in 3D computer graphics, spatial partitioning, geometry processing pipelines, and data structures (KD-Tree, Half-Edge Mesh, Voxel Grids).
- **Algorithm Comparison Testbed**: Allow users to switch between or compare different surface reconstruction algorithms (e.g., Ball-Pivoting Algorithm, Signed Distance Field + Marching Cubes, Alpha Shapes) on imported point clouds, visually inspecting differences in topology, runtime performance, and mesh quality.
- **Modern C++ Craftsmanship**: Demonstrate C++20 design patterns, RAII, modular CMake target design, cache-conscious data layout, clean object-oriented architecture (Strategy & Factory patterns), and automated testing.

---

## 2. Multi-Algorithm Reconstruction Architecture

To support switching between and comparing different algorithms seamlessly, the architecture uses the **Strategy Pattern** with an extensible algorithm registry:

```
                      +-----------------------------+
                      |   ISurfaceReconstruction    |
                      +-----------------------------+
                      | + reconstruct(...) -> Mesh  |
                      | + getParameters()           |
                      | + getName()                 |
                      +-----------------------------+
                                     ^
                                     |
         +---------------------------+---------------------------+
         |                                                       |
+-------------------+                                   +-------------------+
|  BallPivotingAlgo |                                   | MarchingCubesSDF  |
|  (Explicit Mesh)  |                                   |  (Implicit Mesh)  |
+-------------------+                                   +-------------------+
         |                                                       |
         v                                                       v
- Rolls ball of radius R                                - Discretizes space into grid
- Preserves original sample points                      - Computes signed distance (SDF)
- Handles sharp features well                           - Generates watertight manifold mesh
```

### Viewer Comparison Capabilities
1. **Algorithm Switcher & Parameter Inspector**:
   - Live dropdown in Dear ImGui to switch active reconstruction strategy.
   - Dynamic parameter controls tailored to the selected algorithm (e.g., Ball radius $\rho$, cluster tolerance, voxel grid resolution $N^3$, iso-value threshold).
2. **Comparison & Benchmark Panel**:
   - Side-by-side or toggle view between raw point cloud, algorithm output mesh, and wireframe overlay.
   - Profiling metrics: Execution time (ms), triangle count, vertex count, non-manifold edge detection, and memory consumption.
3. **Diagnostic Visual Overlays**:
   - Surface normal orientation needles.
   - Voxel grid boundary and bounding box visualization.
   - Highlight boundary / open edges in the generated mesh.

---

## 3. Technology Stack

| Component | Choice | Reason |
| :--- | :--- | :--- |
| **Language** | C++20 | Modern features (`std::span`, concepts, `std::filesystem`, structured bindings). |
| **Graphics API** | Modern OpenGL 4.1 Core | Native on macOS; lightweight, high performance, cross-platform. |
| **Windowing & Input** | GLFW 3.4 | Stable window creation, high-DPI scaling, and input events. |
| **OpenGL Loader** | GLAD | Generates exact OpenGL 4.1 Core headers and bindings. |
| **GUI** | Dear ImGui (Docking) | Immediate-mode UI for docking panels, sliders, and metrics. |
| **Math** | GLM + Eigen3 | **GLM** for camera/transform math; **Eigen** for SVD/PCA normal estimation. |
| **Build System** | Modern CMake (>= 3.24) | Target-based architecture using `FetchContent` for easy setup. |
| **Unit Testing** | Catch2 v3 | Clean, expressive test framework for geometric algorithms. |

---

## 4. Directory Structure

```
3d-point-cloud-viewer/
├── CMakeLists.txt
├── .gitignore
├── assets/                       # Sample point cloud datasets (.ply, .xyz)
├── docs/                         # Documentation and architectural specs
├── shaders/                      # GLSL vertex & fragment shaders
│   ├── point.vert / point.frag
│   ├── mesh.vert / mesh.frag
│   └── debug_lines.vert / frag
├── src/
│   ├── core/                     # Logging, high-res timer, thread pool
│   ├── geometry/                 # PointCloud, Mesh, KD-Tree, Half-Edge
│   ├── io/                       # File loaders/exporters (PLY ASCII/Binary, OBJ)
│   ├── algorithms/               # Geometry processing & reconstruction
│   │   ├── NormalEstimator.hpp   # k-NN PCA normal estimation
│   │   ├── OutlierRemoval.hpp    # Statistical outlier removal (SOR)
│   │   └── reconstruction/
│   │       ├── IReconstruction.hpp
│   │       ├── BallPivoting.hpp/.cpp
│   │       ├── MarchingCubes.hpp/.cpp
│   │       └── ReconstructionRegistry.hpp
│   ├── render/                   # OpenGL abstraction (VAO, VBO, Shader, Camera, Renderer)
│   └── ui/                       # ImGui UI panels (Viewport, Control, Comparison)
└── tests/                        # Catch2 unit tests for KD-Tree, I/O, Algorithms
```

---

## 5. Development & Learning Roadmap

### Phase 1: Environment & Core Engine Foundation
- Setup CMake project with `FetchContent` (GLFW, GLAD, GLM, ImGui, Catch2).
- Implement OpenGL 4.1 Core context and modern window loop.
- Implement an interactive **Arcball / Orbit Camera** (Rotate, Pan, Zoom).
- Integrate Dear ImGui docking layout.

### Phase 2: Point Cloud I/O & High-Performance Rendering
- Implement PLY file parser (ASCII and Binary formats).
- Build the `PointCloud` data structure (positions, colors, normals, intensity).
- Write GPU shaders for point rendering (custom point sizes, color by height, color by normal).

### Phase 3: Spatial Acceleration & Normal Estimation
- Implement a 3D **KD-Tree** from scratch with $k$-NN radius/nearest search.
- Implement **Normal Estimation** using PCA (covariance matrix SVD via Eigen).
- Render normal vector needles for debugging normal directions.
- Implement Outlier Removal filter (Statistical Outlier Filter).

### Phase 4: Surface Reconstruction Algorithms (Multi-Algorithm Framework)
- Design and implement the `ISurfaceReconstruction` interface.
- **Algorithm A: Ball Pivoting Algorithm (BPA)** (Explicit meshing).
- **Algorithm B: Implicit Voxel SDF + Marching Cubes** (Volumetric meshing).
- Add UI controls to adjust parameters and trigger real-time reconstruction.

### Phase 5: Mesh Processing, Comparison UI & Benchmarks
- Half-Edge mesh representation or indexed triangle structure.
- Add mesh shading modes: Flat, Smooth (Blinn-Phong), Wireframe overlay.
- Build the Side-by-Side / Multi-Algorithm comparison panel with runtime metrics.
- Write unit tests for KD-tree search, normal calculations, and reconstruction edge cases.

### Phase 6: Showcase Polish & Documentation
- Comprehensive `README.md` with architecture diagrams, comparison screenshots, and interactive video/GIF demos.
- Release sample models (Stanford Bunny, Dragon, custom LiDAR / synthetic scans).
