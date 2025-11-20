# 3D_Reconstruction_Using_Sfm

## Introduction
This project implements a complete Structure-from-Motion (SfM) pipeline to reconstruct a sparse 3D point cloud of a Buddha statue from a set of 2D images. The pipeline is built from scratch using classical multi-view geometry, with emphasis on understanding each stage-feature extraction, epipolar geometry, triangulation, track construction, and bundle adjustment. This was done as a part of Coursework for the EECE 7150 (Autonomous Field Robotics) course at Northeastern University. 

## Objective
The primary objective of this project is to:
- Recover camera poses and 3D structure directly from unordered images.
- Build a working SfM pipeline without relying on libraries like COLMAP.
- Implement and analyze each step of incremental reconstruction.
- Perform global bundle adjustment using GTSAM to refine the structure.

## Approach

### 1. Keypoint Detection & Matching
- Extract SIFT features.
- Perform descriptor matching using ratio test and cross-checking.
- Build match pairs for all overlapping images.

### 2. Two-View Initialization
- Select the best image pair using match count and geometric consistency.
- Estimate the Essential matrix using RANSAC (normalized 8-point).
- Decompose E into four (R, t) candidates.
- Use cheirality, parallax, and triangulation quality to choose the valid camera pose.

### 3. Triangulation & Track Building
- Triangulate points using linear DLT.
- Merge all observations of the same feature into multi-view tracks.
- Discard points with poor triangulation angle or high reprojection error.

### 4. Incremental Multi-View Reconstruction
For each new image:
- Match 2D features to existing 3D points.
- Estimate pose using PnP + RANSAC.
- Triangulate additional points.
- Continuously update and expand the global track graph.

### 5. Bundle Adjustment (BA)
Bundle adjustment is performed using GTSAM:
- Optimize all camera poses and 3D landmarks jointly.
- Use robust Huber loss on reprojection factors.
- Fix the first camera pose to remove gauge freedom.
- Compare RMSE before and after optimization.

### Note: The value of the intrinsic matrix 'K' was taken from COLMAP.

## Results
- A sparse 3D reconstruction of the Buddha statue.
- Correctly recovered camera trajectory around the object.
- Reduced reprojection error after BA, improving geometric consistency.

### Sparse 3D Reconstruction Before Bundle Adjustment

<img width="656" height="613" alt="Screenshot from 2025-11-20 15-18-27" src="https://github.com/user-attachments/assets/0edbee6b-9972-43a4-8cc1-8894d166638c" />


### Sparse 3D Reconstruction After Bundle Adjustment

<img width="735" height="644" alt="Screenshot from 2025-11-20 15-18-35" src="https://github.com/user-attachments/assets/6cae5729-243b-4424-9ef5-dc174a8c4c84" />
