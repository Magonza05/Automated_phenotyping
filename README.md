# Automated phenotyping and prediction models for plant growth

This repository contains the image preprocessing, leaf-counting and evaluation notebooks associated with the study **“Automated phenotyping and prediction models for plant growth: A data-driven approach.”** It provides two image-based leaf-counting methods and notebooks for comparing their estimates with manual reference counts.

## Code and execution order

| File | Purpose |
| --- | --- |
| [`Unet -Background Removal + both workflows.ipynb`](Unet%20-Background%20Removal%20%2B%20both%20workflows.ipynb) | **Start here.** Background removal and both leaf-counting methods for the plant images. |
| [`Accuracy Computation.ipynb`](Accuracy%20Computation.ipynb) | Method 1 accuracy calculations against manually recorded counts. |
| [`Leaf Count Method 2 accuracy.ipynb`](Leaf%20Count%20Method%202%20accuracy.ipynb) | Method 2 accuracy calculations against manually recorded counts. |
| [`Performance Evaluation Method 1 and Method 2 .ipynb`](Performance%20Evaluation%20Method%201%20and%20Method%202%20.ipynb) | Comparative evaluation, error metrics and visualisations for both methods. |
| [`Plant Workflow_Region Growing Approach.ipynb`](Plant%20Workflow_Region%20Growing%20Approach.ipynb) | Supplementary Babyroom implementation of the two counting methods; not required when using the main pipeline on Balcony images. |

## Processing methods

The main notebook uses `rembg` to remove the background from cropped plant photographs. Its name refers to the background-removal workflow; it does **not** train a U-Net model.

- **Method 1 — Canny-based counting:** Gaussian smoothing, morphological reconstruction, Canny edge detection, dilation/erosion and connected-component labelling. The number of labelled components is reported as the estimated leaf count.
- **Method 2 — gradient/marker-based counting:** Gaussian smoothing, Sobel gradients calculated for each RGB channel, Otsu thresholding and local-maximum detection within the resulting foreground mask. The number of foreground markers is reported as the estimated leaf count.

## Running the analysis

The original notebooks were developed in **Google Colab** and read images and spreadsheets from Google Drive.

1. Obtain the cropped plant images and manual reference counts from the dataset linked below, and place them in Google Drive.
2. Open the **main pipeline** notebook in Colab. Mount Drive and replace the hard-coded input and output paths with the locations of your copies. Run the relevant Balcony or Babyroom background-removal cells, then point the counting cells to that background-removed image directory.
3. Run the Method 1 and Method 2 counting sections. They export per-image estimated counts to CSV, including plant identifier, acquisition date and time.
4. Open the accuracy and comparison notebooks, update their spreadsheet paths, and run them to compare predictions against manual counts.

Install any missing dependencies in Colab:

```bash
pip install numpy pandas scipy scikit-image scikit-learn matplotlib pillow opencv-python rembg onnxruntime openpyxl reportlab
```

**Evaluation inputs:** The original notebooks refer to `Leaf_count_original.xlsx`, `Leaf_count_Method1_sorted.xlsx`, `Leaf Count _Original M2.xlsx`, `Leaf_count_method2_corrected.xlsx`, `Leaf_count_Method1_and original comparison.xlsx` and `Leaf_count_method2_Original _comparison.xlsx`. The comparison notebook also reads `Results A1 comparison.xlsx`. Use the corresponding reference and generated-count workbooks from the dataset; adjust the paths in each notebook.

## Data and scope

**Images and reference-count spreadsheets:** ADD PUBLIC DATASET LINK HERE.

Example image filename: `plant1_01_06_2023_15_balcony.jpg` (`plant`, day, month, year, acquisition hour, camera/location).

This repository documents the **leaf-counting component** of the study. The separate surface-area and ARIMA/Bayesian forecasting analyses are not implemented in the five notebooks listed above.
