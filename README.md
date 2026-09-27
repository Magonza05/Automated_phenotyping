# Automated Phenotyping and Plant Growth Analysis

This repository contains the image-processing and leaf-counting code used in the study:

**Automated phenotyping and prediction models for plant growth: A data-driven approach**

The repository provides the image preprocessing workflow, two leaf-counting approaches, and the notebooks used to evaluate their performance against manually recorded leaf counts.

## Repository structure

```text
Automated_phenotyping/
│
├── README.md
├── requirements.txt
│
├── notebooks/
│   ├── 01_leaf_count_pipeline.ipynb
│   ├── 02_evaluate_method1.ipynb
│   ├── 03_evaluate_method2.ipynb
│   └── 04_compare_methods.ipynb
│
├── data/
│   └── README.md
│
├── results/
│
└── archive/
    └── babyroom_leaf_count_workflow.ipynb

# Main analysis workflow
The main notebook is:
notebooks/01_leaf_count_pipeline.ipynb
It contains the preprocessing and leaf-counting workflow used for the plant images.
The processing sequence is:
1. Load cropped RGB plant images.
2. Remove the image background using rembg.
3. Apply two alternative leaf-counting approaches.
4. Save the estimated leaf counts to CSV files.
Method 1: Canny-based leaf counting
The first method applies:
- Gaussian smoothing
- morphological reconstruction
- grayscale conversion
- Canny edge detection
- morphological dilation and erosion
- connected-component labelling
The number of connected components is used as the estimated leaf count.
Method 2: gradient- and marker-based leaf counting
The second method applies:
- Gaussian smoothing
- Sobel gradients independently to the RGB channels
- combination of the three gradient maps
- grayscale conversion
- Otsu thresholding to generate a plant mask
- local-maximum detection within the plant region
The number of foreground markers is used as the estimated leaf count.
Evaluation notebooks
02_evaluate_method1.ipynb
Evaluates Method 1 predictions against manually recorded leaf counts.
The notebook computes metrics including:
- Mean Absolute Error (MAE)
- Root Mean Squared Error (RMSE)
- Difference in Count (DiC)
It also generates plant-level visual comparisons between estimated and reference counts.
03_evaluate_method2.ipynb
Evaluates the predictions produced by Method 2 against manually recorded leaf counts.
The notebook merges reference and generated counts using:
- plant identifier
- acquisition date
- acquisition time
and calculates count differences and error metrics.
04_compare_methods.ipynb
Contains the comparative analysis of the two leaf-counting approaches.
It includes:
- average count differences
- error-rate analysis
- plant-level accuracy comparisons
- MAE
- RMSE
- comparison plots
Data
The analysis expects cropped plant images and manually recorded reference leaf counts.
The original study contains images acquired from different camera locations, including Balcony and Babyroom image sets.
Dataset location:
[ADD PUBLIC DATASET LINK HERE]
Additional information about the expected file structure and filenames is provided in data/README.md.
Example image naming convention: plant1_01_06_2023_15_balcony.jpg
The filename contains: plant_<day>_<month>_<year>_<time>_<camera>

The notebooks use these filename components to associate image-level predictions with acquisition metadata.
Running the notebooks
The notebooks were developed in Google Colab and use Google Drive paths.
Option 1: Google Colab
1. Upload or copy the repository notebooks to Google Colab.
2. Mount Google Drive when prompted.
3. Place the image dataset in your Drive.
4. Update the input and output directory variables near the beginning of the notebook.
5. Run the cells sequentially.
For example:

