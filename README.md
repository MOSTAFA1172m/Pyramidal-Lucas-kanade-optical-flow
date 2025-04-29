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
   ```
### Methodology



#### Data Preparation



Dataset: Dimetrodon sequence from the Middlebury Vision repository, consisting of two frames (`im0.png`, `im1.png`) with slight motion.



Images are loaded in grayscale and normalized to `[0, 1]`.



### Gradient Computation


Sobel operators are used to compute image gradients (`Ix`, `Iy`) in the x and y directions:



```math

G_x = \begin{bmatrix} -1 & 0 & 1 \\ -2 & 0 & 2 \\ -1 & 0 & 1 \end{bmatrix}, \quad G_y = \begin{bmatrix} -1 & -2 & -1 \\ 0 & 0 & 0 \\ 1 & 2 & 1 \end{bmatrix}

```
Temporal gradient (`It`) is computed as the difference between warped and original frames.


### Image Pyramid Construction


A Gaussian pyramid is built with a scaling factor of 0.5 per level (typically 3–4 levels).



The `build-pyramid` function downsamples images using `cv2.pyrDown` and reverses the list for coarse-to-fine processing.



### Pyramidal Lucas-Kanade Algorithm


Optical flow is estimated starting at the coarsest pyramid level and refined at finer levels.



At each level:





Flow from the previous level is upsampled using cv2.pyrUp and scaled by 2.



Gradients (`Ix`, `Iy`) are computed for the current level.



The second image is warped using current flow estimates.



Flow updates are computed via least-squares:




```math
A = \begin{bmatrix} I_x & I_y \end{bmatrix}, \quad b = -I_t, \quad \mathbf{A}^T \mathbf{A} \nu = \mathbf{A}^T \mathbf{b}
```

Iterations (typically 3) refine the flow estimates.

Visualization





Flow fields (`u`, `v`) are visualized using quiver plots, with arrows indicating motion direction and magnitude.



An exaggeration factor (e.g., 50) is applied for better visibility.



### Results





The algorithm successfully estimated motion in the Dimetrodon sequence, capturing both small and large displacements.



Quiver plots demonstrated accurate motion fields, outperforming single-scale Lucas-Kanade methods for larger motions.



Visualizations confirmed the effectiveness of the pyramidal approach.




### Future Improvements





Incorporate feature-based tracking (e.g., Shi-Tomasi corner detection) for enhanced robustness.



Optimize for real-time performance using parallel processing or GPU acceleration.



Explore advanced warping techniques or robust cost functions.

### References





B. D. Lucas and T. Kanade, "An iterative image registration technique with an application to stereo vision," Proceedings of Imaging Understanding Workshop, 1981.



J. Y. Bouguet, "Pyramidal Implementation of the Lucas-Kanade Feature Tracker," Intel Corporation, 1999.



R. Szeliski, Computer Vision: Algorithms and Applications, Springer, 2010.



J. Barron, D. Fleet, and S. Beauchemin, "Performance of Optical Flow Techniques," International Journal of Computer Vision, vol. 12, no. 1, pp. 43–77, 1994.




### Authors





Mostafa Hazem Mostafa



Khaled Walid Ghalwash

### License

This project is licensed under the MIT License - see the LICENSE file for details.
