# Multi-View 3D Object Detection Network

## Overview
- Network input: LIDAR point cloud and RGB images
- Network output: oriented 3D bounding box
- Idea: use multimodal info to perform region-based feature fusion.

## Representation Encoding
- Encode the sparse 3D point cloud with a multi-view representation.
- Using bird's eye view representation of 3D point cloud.
    - it can be projected onto any views in 3D space.
- Fusion of LIDAR point cloud and RGB image (more detailed semantic information).


## Network Architecture
- Composed of two subnetworks: one for 3D object proposal generation and another for multi-view feature fusion.
- The network extracts region-wise features by projecting 3D proposals to the feature maps from multiple views.
- Deep fusion scheme to combine region-wise features from multiple views, enabling interactions between intermediate layers of different paths.


## Bird's eye view representation
- Encoded by height, intensity and density
- Discretize the projected point cloud into a 2D grid
- For each cell, the height feature is computed as the maximum height of the points in the cell
- The point cloud is divided equally into M slices. A height map is computed for each slice

### Advantages
- Object preserve physical sizes when projected to the bird's eye view
- Objects occupy different space thus avoiding occlusion problem
- Since objects lie on the ground plane and have small variance in vertical location, the bird's eye view location is more crucial to get the 3D BBs

## Front view representation
- Complementary info to bird's eye view
- LIDAR point cloud is very sparse, projecting it into image plane results in sparse 2D point map. Instead, project to a cylindar plane to generate a dense front view map

## Feature map upsampling
- 2x bilinear upsampling after the last conv layer
- The front-end convolutions only proceed three pooling operations 8x downsampling
- Combined with 2x deconvolution, the feature map fed to the network is 4x downsampled w.r.t. to the bird's eye view


## Regularization
### Drop-path
- This is simply finding connections/routes down the network and cutting the connections temporarily

### Auxiliary Loss

