# Salmon Scale Ring Analysis

## Objective
Develop an automated solution to analyze salmon scale images by:
- Detecting the center of the scale
- Measuring distances from the center to the outermost ring
- Counting and measuring distances between visible growth rings

## Data Source
- In-house salmon scale images provided by PSF (Pacific Salmon Foundation).
- Grayscale `.tif` images of varying quality and scale structure.

## Methods & Process

### 1. Edge Detection
- Used the Canny Edge Detection algorithm via `cv2.Canny(image)` to extract edges from grayscale images.
- Canny is a multi-stage edge detector that finds the boundaries of objects by identifying intensity gradients.
- `cv2` refers to OpenCV, a widely-used open-source computer vision library in Python.

### 2. Center Detection (`detect_center.ipynb`)

#### Hough Circle Transform
- Applied `cv2.HoughCircles` to detect ring-like patterns.
- Detected circles were drawn and visualized on the original image.
- The center was estimated by:
  - Extracting circle centers
  - Clustering using DBSCAN
  - Averaging cluster centroids to find the final derived center

```python
cv2.HoughCircles(edges, cv2.HOUGH_GRADIENT, dp=1.5, minDist=50, param1=90, param2=35)
```

### 3. Hyperparameter Tuning (`center_hyperparam_tuning.ipynb`)
- Performed random search across:
  - `dp`, `minDist`, `param1`, `param2` for cv2.HoughCircles
  - `eps`, `min_samples` for DBSCAN
- Chose parameter sets that minimized distance error from known ground-truth centers.
- Reported best parameters and detection accuracy per image.

### 4. Ring Transect Analysis (plot_transect.ipynb)
- Visualized edge image with center overlay.
- Computed distance from center to outermost edge point.
- Drew a transect line from center to edge to guide future ring analysis.

## Outputs
- Final detected center coordinates for each image
- Distance from center to outer ring
- Overlay plots for center and transect verification
- Optimal hyperparameter report per image

## Next Steps
- Automate ring counting and spacing analysis along the transect
- Classify scale patterns by species or life-history type
