# InteriorGS: 3D Gaussian Splatting Dataset of Semantically Labeled Indoor Scenes

A comprehensive indoor scene dataset featuring 3D Gaussian representations with semantic annotations and spatial occupancy information.

<div align="center" style="margin-top: 0; padding-top: 0; line-height: 1; padding-bottom: 10px;">
    <a href="https://github.com/manycore-research/InteriorGS" target="_blank" style="margin: 2px;"><img alt="Project"
    src="https://img.shields.io/badge/GitHub-InteriorGS-24292e?logo=github&logoColor=white" style="display: inline-block; vertical-align: middle;"/></a>
    <a href="https://huggingface.co/datasets/spatialverse/InteriorGS" target="_blank" style="margin: 2px;"><img alt="arXiv"
    src="https://img.shields.io/badge/%F0%9F%A4%97%20Dataset-InteriorGS-ffc107?color=ffc107&logoColor=white" style="display: inline-block; vertical-align: middle;"/></a>
</div>

<div align="center">
  <image src="https://kloudsim-usa-cos.kujiale.com/InteriorGS/InteriorGS_overview2.jpg" alt="InteriorGS Overview" style="max-width: 100%; height: auto;"/>
  <p><i>Sample from the InteriorGS dataset. The dataset provides high-quality 3D Gaussian Splatting (3DGS) representations along with instance-level semantic bounding boxes and occupancy maps indicating agent-accessible areas. The red and yellow trajectories denote the paths of a ground robot and a UAV (drone), respectively. Notably, InteriorGS supports free-form agent navigation and interaction within a continuous 3D environment, enabling realistic spatial intelligence training and evaluation.</i></p>

  <video src="https://github.com/user-attachments/assets/a310c61e-d47a-45c3-9e88-ba38e1e9ee95"> </video>
  <p><i>🤖RGB rendering and corresponding depth map from the perspective of the human/ground robot along the red trajectory</i></p>

  <video src="https://github.com/user-attachments/assets/f4dd362f-4138-4bd7-a91b-42a64f24790e"> </video>
  <p><i>🚁RGB rendering and corresponding depth map from the UAV (drone) perspective along the yellow trajectory.</i></p>
</div>

## 🔄 News

- 2025-08-01: Released InteriorGS v2.0, featuring 1,000 indoor scenes along with their corresponding floorplans.
- 2025-07-24: Initial release InteriorGS (v1.0) with 500 scenes.

## 📋 Overview

Existing 3D indoor scene datasets mainly fall into two categories, each with its own limitations. Datasets reconstructed from RGB-D scans are typically represented as triangle meshes, but often suffer from incomplete geometry, occlusions, and missing semantic or structural annotations. Due to the incompleteness of the reconstructed scenes, they do not support continuous agent movement, limiting their use in navigation and interaction tasks.

In contrast, artist-designed 3D scenes provide high visual quality, but are available in limited numbers and often lack consistent semantic and spatial annotations, making them less suitable for large-scale scene understanding.

To overcome these issues, we present InteriorGS, a dataset of 1,000 indoor scenes represented using 3D Gaussian Splatting (3DGS). We create it by photorealistically rendering handcrafted 3D environments, then reconstructing splatting-based representations from over 5 million images, resulting in high-quality and lightweight 3D data that supports real-time rendering. Each scene is semantically annotated at the object level, with more than 554,000 object instances across 755 categories, and each object is paired with a 3D bounding box. We also provide an occupancy map for each scene to support navigation and spatial understanding.

InteriorGS includes over 80 types of indoor environments, such as homes, convenience stores, wedding banquet halls, and museums. It supports various downstream tasks, including 3D scene understanding, controllable scene generation, and embodied agent navigation, and offers a valuable resource for research in spatial intelligence.

We provide a [sample viewer](https://www.kujiale.com/pub/koolab/koorender/InteriorGS) for viewing our data.

## 🗂️ Dataset Structure

InteriorGS dataset is released on the [🤗 Huggingface](https://huggingface.co/datasets/spatialverse/InteriorGS) and is organized as follows:

```bash
InteriorGS
├── 0001_839920             # 3D Gaussian scene with detailed information
│   ├── 3dgs_compressed.ply # Gaussian point cloud files
│   ├── labels.json         # Semantic annotations and bounding boxes
│   ├── occupancy.png       # Grayscale occupancy map
│   └── occupancy.json      # Occupancy metadata
|   └── structure.json      # Floorplan data
├── 0002_839955
│   ├── 3dgs_compressed.ply
│   ├── labels.json
│   ├── occupancy.png
│   └── occupancy.json
|   └── structure.json 
└── ...
```

## 📊 Data Description

### 3D Gaussian Models (.ply files)

- **Format**: PLY (Polygon File Format)
- **Content**: 3D Gaussian parameters including position, covariance, opacity, and spherical harmonics coefficients. The data is **compressed** using the **SuperSplat** method to optimize storage and loading efficiency.
- **Coordinate System**: The coordinate system of the scenes is defined as **XYZ = (Right, Back, Up)**
- **Units**: Meters

### Semantic Annotations

- **Bounding Boxes**: 3D oriented bounding boxes defined by 8 corner vertices
- **Labels**: Per-object semantic class labels with instance IDs
- **Format**: JSON files containing arrays of annotated objects

### Occupancy Maps

- **Resolution**: 1024x1024 pixels (configurable)
- **Format**: PNG (grayscale) + JSON (metadata). Metadata is used for mapping between the 2D pixels and 3DGS world coordinates.
- **Coverage**: Top-down view of navigable floor space
- **Values**: 
  - `255` (white): Free space
  - `0` (black): Occupied space
  - `127` (gray): Unknown space

### Floorplan data (structure.json)

This document provides an explanation of the keys used in the scene layout data. Coordinate system is right-handed, z
up, and unit in meter.

- **rooms**: array of rooms within the scene layout. Virtual partition of the scene that does not refer to actual geometry.
    - **profile**: 2D polygon in xy plane that defines room outline or contour.
    - **room_type**: functional type, such as "Living and Dining Room", "Master Bedroom", etc.

- **walls**: array of wall meshes.
    - **location**: line segment in xy plane.
    - **thickness**: total distance extended from line segment in direction in xy plane and orthogonal to line segment .
    - **height**: height in z axis from ground(z=0).

- **holes**: openings on the walls, for placing other objects such as window and door.
    - **profile**: 3D polygon that defines the hole shape on the wall.
    - **thickness**: total distance extended from profile in direction of profile plane normal.
    - **type**: WINDOW or DOOR.

- **ins**: instances within scene.
    - **label**: semantic label, such as "Window", "Curtain", "Chair", etc.
    - **position**: instance bounding box center in world frame.
    - **size**: instance bounding box size in world frame.

## 🏠 Citation

If you use **InteriorGS** in your research or development, please cite or link to our project page:

```bibtex
@misc{InteriorGS2025,
  title        = {InteriorGS: A 3D Gaussian Splatting Dataset of Semantically Labeled Indoor Scenes},
  author       = {SpatialVerse Research Team, Manycore Tech Inc.},
  year         = {2025},
  howpublished = {\url{https://huggingface.co/datasets/spatialverse/InteriorGS}}
}
```

## 📄 License

This dataset is released under [InteriorGS License](https://kloudsim-usa-cos.kujiale.com/InteriorGS/InteriorGS_Terms_of_Use.pdf).

## 🙏 Acknowledgements

This project makes partial use of code from the [SuperSplat](https://github.com/playcanvas/supersplat) repository. We thank the authors for open-sourcing their implementation.
