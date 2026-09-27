# Automated phenotyping and prediction models for plant growth

Code accompanying the lettuce leaf-counting component of *Automated phenotyping and prediction models for plant growth: A data-driven approach*.

## Repository contents

| Notebook | Purpose |
| --- | --- |
| [`Unet -Background Removal + both workflows.ipynb`](notebooks/historical/Unet%20-Background%20Removal%20%2B%20both%20workflows.ipynb) | Background removal with `rembg` and batch leaf counting for Balcony images. |
| [`Plant Workflow_Region Growing Approach.ipynb`](notebooks/historical/Plant%20Workflow_Region%20Growing%20Approach.ipynb) | Batch leaf counting for Babyroom images using both methods. |
| [`Performance Evaluation Method 1 and Method 2 .ipynb`](notebooks/historical/Performance%20Evaluation%20Method%201%20and%20Method%202%20.ipynb) | Comparison of estimated counts with manually recorded counts. |
| [`Leaf Count Method 2 accuracy.ipynb`](notebooks/historical/Leaf%20Count%20Method%202%20accuracy.ipynb) | Additional evaluation of the second counting method. |

## Leaf-counting methods

- **Method 1 — Canny:** Gaussian smoothing, morphological reconstruction, Canny edge detection, dilation/erosion and connected-component counting.
- **Method 2 — marker detection:** Gaussian smoothing, combined RGB Sobel gradients, an Otsu binary mask and local-maxima marker counting.

These are historical implementations, shared with their original filenames. They are not a one-click reproduction; verify the exported counts against the reference data.

## Running the notebooks

1. Open a notebook in **Google Colab**, mount Google Drive and update its input/output paths to your local copies of the images and reference files.
2. Run the background-removal cells, followed by the relevant Balcony or Babyroom counting cells. The counting notebooks export image-level leaf-count estimates to CSV.
3. Open the evaluation notebooks with the prediction CSVs and matching manual-count workbooks to inspect the count comparisons.

The notebooks use Python packages including `numpy`, `pandas`, `scipy`, `scikit-image`, `opencv-python`, `Pillow`, `rembg`, `matplotlib`, `openpyxl` and `reportlab`. Some cells have notebook-specific installation commands.

## Data

**Plant photographs and manual reference counts:** [insert public dataset link before release].

The original notebooks use Google Drive paths, so those paths must be changed to the downloaded dataset location. This release covers the image-processing and leaf-counting component; the separate surface-area and growth-forecasting analysis is not included in these notebooks.
