# Automated phenotyping and prediction models for plant growth

Image-processing and leaf-counting code accompanying *Automated phenotyping and prediction models for plant growth: A data-driven approach*.

## Code

[`Unet -Background Removal + both workflows.ipynb`](Unet%20-%20Background%20Removal%20%2B%20both%20workflows.ipynb) is the main Google Colab notebook. It contains background removal for cropped Balcony and Babyroom plant images and the two batch leaf-counting methods evaluated in the image-analysis experiment:

- **Method 1 (edge-based):** Gaussian smoothing, morphological reconstruction, Canny edge detection, dilation/erosion and connected-component counting.
- **Method 2 (marker-based):** Gaussian smoothing, RGB-channel gradient calculation, Otsu thresholding and local-maxima marker counting.

The notebook uses `rembg` for background removal. Its filename does not indicate that a U-Net model was trained in this study.

## Running in Google Colab

1. Open the notebook in Colab and mount Google Drive. Install the dependencies specified in the notebook (`rembg`, `scikit-image`, `scipy`, `Pillow` and supporting Python packages).
2. Set the input/output folders in the **Balcony** or **Babyroom Images Background Removal** section to the corresponding cropped images. Run the relevant background-removal cells.
3. Set `image_folder_path` in **First Approach – Edge + Watershed** to the background-removed image folder. Run the complete batch-counting cell that writes `leaf_counts_balcony.csv`. Despite the historical heading, this particular batch cell counts connected components; it does not execute watershed.
4. Set `folder_path` in **2nd Approach – Gaussian Filter + Sobel + Markers + Otsu** to the same image folder. Run the batch cell that writes `leaf_count_results_markers_balcony.csv`. Change output filenames if running on Babyroom images.

The notebook includes alternative experimental cells; the steps above identify the batch-processing path. Its Babyroom background-removal cell currently selects filenames beginning `plant5` or `plant6`; adjust that condition when processing other plants.

## Data and outputs

**Image dataset and manual reference counts:** [ADD PUBLIC DATASET URL BEFORE RELEASE]

Input filenames contain plant ID, date, acquisition time and camera/location. The two batch methods export per-image estimated counts to CSV for comparison with manual reference counts.

This notebook covers background removal **from already cropped plant images** and leaf-count estimation. The earlier raw-image cropping stage, and the separate surface-area and forecasting analyses, are not included here.
