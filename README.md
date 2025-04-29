# Pyramidal Lucas-Kanade Optical Flow

## Project Overview
This project implements the Pyramidal Lucas-Kanade Optical Flow algorithm to estimate motion between consecutive video frames. Using image pyramids and iterative refinement, it tracks pixel displacements across multiple resolutions, handling small and large motions. The implementation leverages Python, OpenCV, and NumPy, with motion fields visualized via quiver plots. The project was tested on the Dimetrodon sequence from the Middlebury Vision repository.

## Features
- Computes optical flow with the Lucas-Kanade method in a pyramidal framework.
- Supports multi-scale analysis for robust motion estimation.
- Visualizes flow fields with exaggerated motion for clarity.
- Includes Sobel-based gradient computation and image warping.

## Installation
### Prerequisites
- Python 3.6+
- OpenCV (`cv2`)
- NumPy
- Matplotlib

### Setup
1. Clone the repository:
   ```bash
   git clone https://github.com/MOSTAFA1172m/Pyramidal-Lucas-kanade-optical-flow.git
   cd Pyramidal-Lucas-kanade-optical-flow


   
### Methodology



#### Data Preparation



Dataset: Dimetrodon sequence from the Middlebury Vision repository, consisting of two frames (`im0.png`, `im1.png`) with slight motion.



Images are loaded in grayscale and normalized to `[0, 1]`.



### Gradient Computation


Sobel operators are used to compute image gradients (Ix, Iy) in the x and y directions:



```math

G_x = \begin{bmatrix} -1 & 0 & 1 \\ -2 & 0 & 2 \\ -1 & 0 & 1 \end{bmatrix}, \quad G_y = \begin{bmatrix} -1 & -2 & -1 \\ 0 & 0 & 0 \\ 1 & 2 & 1 \end{bmatrix}






