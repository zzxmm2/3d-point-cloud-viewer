# 3D Point Cloud Viewer & Surface Reconstruction

An interactive 3D point cloud visualizer and surface reconstruction engine built with **C++20**, **OpenGL 4.1 Core**, and **Dear ImGui**.

## Highlights

- **Multi-Algorithm Surface Reconstruction**: Compare different reconstruction approaches (Ball-Pivoting, Implicit SDF + Marching Cubes) on imported point clouds.
- **Modern C++20 Architecture**: Modular CMake target structure, RAII resource management, Strategy pattern for reconstruction algorithms.
- **Interactive 3D Viewport**: Arcball orbit camera, custom shaders, normal needle visualization, wireframe overlays, and live parameter tuning.
- **Computational Geometry from Scratch**: Custom 3D KD-Tree for $k$-NN acceleration, PCA-based normal estimation, and mesh topology processing.

## Documentation

- [Architecture & Implementation Roadmap](file:///Users/zzxmm/Github/3d-point-cloud-viewer/docs/architecture_and_roadmap.md)
