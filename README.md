# Pyramidal Lucas-Kanade Optical Flow
Project Overview
This project implements the Pyramidal Lucas-Kanade Optical Flow algorithm to estimate motion between consecutive video frames. By leveraging image pyramids and iterative refinement, the algorithm tracks pixel displacements across multiple resolutions, handling both small and large motions. The implementation uses Python, OpenCV, and NumPy, with motion fields visualized using quiver plots. The project was tested on the Dimetrodon sequence from the Middlebury Vision repository.
Features

Computes optical flow using the Lucas-Kanade method with a pyramidal approach.
Supports multi-scale analysis for robust motion estimation.
Visualizes flow fields with exaggerated motion for clarity.
Includes gradient computation using Sobel operators and image warping.

Installation
Prerequisites

Python 3.6+
OpenCV (cv2)
NumPy
Matplotlib

Setup

Clone the repository:git clone https://github.com/MOSTAFA1172m/Pyramidal-Lucas-kanade-optical-flow.git
cd Pyramidal-Lucas-kanade-optical-flow


Install dependencies:pip install opencv-python numpy matplotlib


Download the Dimetrodon dataset (or use your own image pairs) and place the images (im0.png, im1.png) in the project directory.

Usage

Ensure the input images (im0.png, im1.png) are grayscale and placed in the project directory.
Run the main script to compute and visualize optical flow:python main.py


The script will:
Load the images.
Compute optical flow using the pyramidal Lucas-Kanade algorithm.
Display a quiver plot showing the motion field with an exaggeration factor of 50.



Example Code
import cv2
import numpy as np
from pyramidal_lucas_kanade import pyramidal_lucas_kanade, draw_quiver

# Load images
I1 = cv2.imread('im0.png', cv2.IMREAD_GRAYSCALE)
I2 = cv2.imread('im1.png', cv2.IMREAD_GRAYSCALE)

# Compute optical flow
u, v = pyramidal_lucas_kanade(I1, I2, levels=4, window_size=15, iterations=3)

# Visualize results
draw_quiver(u, v, step=10, title="Dimetrodon Optical Flow", exaggeration_factor=50)

Methodology
Data Preparation

Dataset: Dimetrodon sequence from the Middlebury Vision repository, consisting of two frames (im0.png, im1.png) with slight motion.
Images are loaded in grayscale and normalized to $[0, 1]$.

Gradient Computation

Sobel operators compute image gradients ($I_x$, $I_y$) in the x and y directions:$$G_x = \begin{bmatrix}-1 & 0 & 1 \-2 & 0 & 2 \-1 & 0 & 1\end{bmatrix}, \quadG_y = \begin{bmatrix}-1 & -2 & -1 \0 & 0 & 0 \1 & 2 & 1\end{bmatrix}$$
Gradients are calculated as:$$S_x(i, j) = \sum_{m=-1}^{1} \sum_{n=-1}^{1} G_x(m, n) \cdot I(i+m, j+n)$$$$S_y(i, j) = \sum_{m=-1}^{1} \sum_{n=-1}^{1} G_y(m, n) \cdot I(i+m, j+n)$$
Temporal gradient ($I_t$) is the difference between warped and original frames.

Image Pyramid Construction

A Gaussian pyramid is built with a scaling factor of 0.5 per level (typically 3–4 levels).
The build_pyramid function downsamples images using cv2.pyrDown and reverses the list for coarse-to-fine processing:def build_pyramid(img, levels):
    pyramid = [img]
    for _ in range(1, levels):
        img = cv2.pyrDown(img)
        pyramid.append(img)
    return pyramid[::-1]



Pyramidal Lucas-Kanade Algorithm

Optical flow is estimated starting at the coarsest pyramid level and refined at finer levels.
At each level:
Flow from the previous level is upsampled using cv2.pyrUp and scaled by 2.
Gradients ($I_x$, $I_y$) are computed for the current level.
The second image is warped using current flow estimates:def warp_image(img, u, v):
    h, w = img.shape
    grid_x, grid_y = np.meshgrid(np.arange(w), np.arange(h))
    map_x = (grid_x + u).astype(np.float32)
    map_y = (grid_y + v).astype(np.float32)
    return cv2.remap(img, map_x, map_y, interpolation=cv2.INTER_LINEAR, borderMode=cv2.BORDER_REFLECT)


Flow updates are computed via least-squares:$$A = \begin{bmatrix} I_x & I_y \end{bmatrix}, \quad b = -I_t, \quad \mathbf{A}^T \mathbf{A} \nu = \mathbf{A}^T \mathbf{b}$$where $\nu = \begin{bmatrix} u \ v \end{bmatrix}$ is the flow vector.
Iterations (typically 3) refine the flow estimates.



Visualization

Flow fields ($u$, $v$) are visualized using quiver plots, with arrows indicating motion direction and magnitude.
An exaggeration factor (e.g., 50) is applied for better visibility:def draw_quiver(u, v, step=10, title="Optical Flow", scale=5, exaggeration_factor=50):
    h, w = u.shape
    y, x = np.mgrid[0:h:step, 0:w:step]
    u_s = u[::step, ::step] * exaggeration_factor
    v_s = v[::step, ::step] * exaggeration_factor
    magnitude = np.sqrt(u_s**2 + v_s**2)
    plt.figure(figsize=(12, 8))
    plt.quiver(x, y, u_s, -v_s, magnitude, angles='xy', scale_units='xy', scale=scale, cmap='jet')
    plt.colorbar(label="Flow Magnitude")
    plt.gca().invert_yaxis()
    plt.title(title)
    plt.show()



Results

The algorithm successfully estimated motion in the Dimetrodon sequence, capturing both small and large displacements.
Quiver plots demonstrated accurate motion fields, outperforming single-scale Lucas-Kanade methods for larger motions.
Visualizations confirmed the effectiveness of the pyramidal approach.

Future Improvements

Incorporate feature-based tracking (e.g., Shi-Tomasi corner detection) for enhanced robustness.
Optimize for real-time performance using parallel processing or GPU acceleration.
Explore advanced warping techniques or robust cost functions.

References

B. D. Lucas and T. Kanade, "An iterative image registration technique with an application to stereo vision," Proceedings of Imaging Understanding Workshop, 1981.
J. Y. Bouguet, "Pyramidal Implementation of the Lucas-Kanade Feature Tracker," Intel Corporation, 1999.
R. Szeliski, Computer Vision: Algorithms and Applications, Springer, 2010.
J. Barron, D. Fleet, and S. Beauchemin, "Performance of Optical Flow Techniques," International Journal of Computer Vision, vol. 12, no. 1, pp. 43–77, 1994.

Authors

Mostafa Hazem Mostafa
Khaled Walid Ghalwash

License
This project is licensed under the MIT License - see the LICENSE file for details.
Notes on Rendering

Code: The Python code blocks above use ```python for syntax highlighting, which should appear colorful (e.g., keywords in orange, strings in green) when viewed on GitHub or platforms supporting GitHub-Flavored Markdown.
Math: LaTeX equations are enclosed in $ (inline) or $$ (block) and will render as formatted math on GitHub. If equations don't render, ensure you're viewing the README on a platform with MathJax support (e.g., GitHub, Jupyter Notebook, or a Markdown viewer like Obsidian with LaTeX plugins).
Other Platforms: If sharing on LinkedIn or platforms without LaTeX/syntax highlighting support (related to your earlier quality concerns), consider:
Embedding screenshots of the rendered README from GitHub.
Converting the README to a PDF with rendered math using tools like Pandoc or a Markdown-to-PDF converter, then uploading the PDF.
Linking directly to the GitHub repository: https://github.com/MOSTAFA1172m/Pyramidal-Lucas-kanade-optical-flow for best viewing.



