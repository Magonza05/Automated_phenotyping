# Lettuce leaf-counting: recovered research code

**Repository status: historical code recovered; end-to-end reproduction is still in progress.** These notebooks are a documented starting point for reconstructing the leaf-counting experiment associated with *Automated phenotyping and prediction models for plant growth: A data-driven approach*. They should **not** yet be treated as a verified, one-click reproduction of the submitted paper's results.

## 1. Scope of this release

This release addresses the **earlier, image-based leaf-counting experiment**: RGB acquisition, isolation/background removal of individual plants, two candidate classical leaf-counting implementations, and comparison with manual reference counts. The manuscript also combines a **separate later experiment** used for projected-area estimation and ARIMA/Bayesian growth forecasting. Its original images, processed area measurements and forecasting code are **not included here**; they must be released and documented separately with the contributor responsible for that experiment.

Original notebook filenames are retained under `notebooks/historical/` to preserve provenance. Their saved notebook outputs were cleared to keep the upload small; **source code cells were not changed**. They contain historical Google Colab/Drive paths, duplicated or unused experiments and some cells that require correction. Do not assume `Runtime > Run all` succeeds.

## 2. Repository contents

```text
README.md
.gitignore
data/
    README.md
notebooks/
    historical/
        Unet -Background Removal + both workflows.ipynb
        Plant Workflow_Region Growing Approach.ipynb
        Performance Evaluation Method 1 and Method 2 .ipynb
        Leaf Count Method 2 accuracy.ipynb
docs/
    NOTEBOOK_AUDIT.md
```

| Notebook | Evidence it provides | Important qualification |
|---|---|---|
| `Unet -Background Removal + both workflows.ipynb` | Batch `rembg` preprocessing, Balcony Canny/component-counting implementation and Balcony Sobel/Otsu/marker counting | The title says “Unet,” but the visible background-removal code calls `rembg`; no trained custom U-Net is established. It includes unused/incomplete cells and hard-coded paths. |
| `Plant Workflow_Region Growing Approach.ipynb` | Corresponding Canny and Sobel/Otsu/local-maxima batch methods on the Babyroom processed-image directory | Despite the title, the visible second batch method counts markers directly; it does not apply region growing or watershed. |
| `Performance Evaluation Method 1 and Method 2 .ipynb` | Historical paired-workbook MAE/RMSE/DiC plots and an exploratory `Results A1 comparison.xlsx` analysis | Includes hard-coded values and fragile/inconsistent variable/column use. Do not copy its “accuracy” values into the paper without recomputing from verified pairs. The A1 benchmark's source and algorithm identity remain unverified. |
| `Leaf Count Method 2 accuracy.ipynb` | Earlier comparison/plotting code using the corrected Method 2 and original-count workbooks | Requires standardized IDs and checks for duplicate/omitted timestamps. Not a substitute for a single clean evaluation script. |

See [`docs/NOTEBOOK_AUDIT.md`](docs/NOTEBOOK_AUDIT.md) for the remaining notebooks, their role and publication recommendations.

## 3. Historical computational pathway (candidate, not yet verified)

```text
Original Balcony/Babyroom group photographs
   |  camera-specific rotation/cropping and four-plant splitting
   |  NOTE: the executable upstream crop/rotation notebook still needs to be recovered
   v
Cropped images with plant ID, date, time and camera in the filename
   |  rembg background removal (alpha matting)
   v
Background-removed images
   |------------------------------|
   v                              v
Method 1                         Method 2
Gaussian smoothing               Gaussian smoothing
Morphological reconstruction     Sobel gradients on R, G and B
Grayscale + Canny                Grayscale Otsu threshold
Dilation, erosion                Local-maxima markers in binary mask
Connected-component labelling   Count foreground markers
   |                              |
   |---- per-image count CSV -----|
                  |
      Align with manual reference counts
                  |
         MAE, RMSE, DiC, error analysis
```

For the **recovered batch implementation**, Method 1 uses Gaussian smoothing (`sigma=1`), grayscale Canny (`sigma=1`), dilation and erosion (disk radius 5) and 8-connected component labelling. The algorithm reports the number of connected **edge components** as its leaf-count estimate; this is not an instance-segmentation ground truth.

Method 2 uses Gaussian smoothing (`sigma=1`), summed Sobel gradients from the three RGB channels, Otsu thresholding and `peak_local_max` with `threshold_rel=0.992`, `min_distance=40` constrained by the binary mask. It reports the **number of foreground markers**. This visible batch code does not contain a subsequent watershed step. Markers in the background and multiple markers on one leaf are possible failure cases.

**The submitted manuscript describes additional/different operations** (e.g. CLAHE, contours and watershed in Method 1; HSV/K-means and watershed in Method 2). Exploratory watershed code exists in other recovered notebooks, but it has not been connected to the batch CSVs or final reported results. The manuscript and public methods must be reconciled *after* reproduction, not retrofitted from notebook titles.

## 4. Data needed for reproduction

The actual photograph database and reference workbooks are **not included in this starter package**. Do not state in the paper that they are public until they have been deposited and their links tested. Publish the images with an open-data host appropriate for their size, provide a permanent citation/DOI when available, and link the dataset here. Keep code and modest metadata in GitHub; do not commit the entire photographic archive by default.

The historical notebooks reference Drive folders including `Balcony Cropped and Quadrants`, `Babyroom Cropped and Quadrants`, `Babyroom_BGR` and `Balcony_BG unet`. A typical individual filename is `plant5_05_06_2023_07_babyroom.png`, indicating plant ID, date, time and camera. Exact dates, which plants were evaluated, and the input-image manifest must be verified against the original data.

The earlier evaluation used workbooks with names resembling `Leaf_count_Method1_and original comparison.xlsx`, `Leaf Count _Original M2.xlsx`, `Leaf_count_method2_corrected.xlsx` and `Leaf_count_method2_Original _comparison.xlsx`. **Raw and corrected references differ:** preserve the originals, publish the correction log and use an explicitly versioned reference set. The later experiment's area and forecasting data should have a separate dataset manifest.

See [`data/README.md`](data/README.md) for the expected public data layout and required metadata.

## 5. Opening the historical notebooks in Colab

1. Obtain the authorized original photographs and matching reference files. Make your own copies of the historical notebooks; preserve the originals without code edits.
2. Start Google Colab and mount Google Drive where the notebooks request it. Update only the absolute input/output folder paths for your copy, recording every change. Some notebooks use Google Drive paths specific to the original author's account.
3. Install the dependencies required by the specific notebook, including `numpy`, `scipy`, `scikit-image`, `opencv-python`, `Pillow`, `rembg`, `matplotlib`, `pandas`, `openpyxl`, and (for older code) `reportlab`. **Versions are not yet locked.** Check the `rembg` model/backend and the `scikit-image` version before comparing counts; their behaviour may vary with release.
4. Run the relevant preprocessing and counting cells in order. The historical notebooks contain demonstration, commented or incomplete cells: **do not assume that running the full notebook top to bottom is supported.** Do not alter thresholds when testing whether the original output can be reproduced.
5. For each image, write a record with `image_id`, `plant_id`, ISO `date`, `time`, `camera`, algorithm version, predicted count and intermediate image paths. Match predictions to the reference data on a validated unique image/plant/date/time key. Report unmatched and duplicate rows; never silently discard them.
6. Independently recompute MAE, RMSE, signed bias and DiC on the same matched observations for both methods. Report the number of matched observations for each method, plus a common-observation comparison. Only then update the manuscript's numerical statements.

### Minimal verification target

Select one published test image for each camera and each method. Confirm that rerunning the historical code reproduces the **historical per-image count** exactly with the recorded environment. Then extend to the complete released sample and require a documented explanation for every mismatch. Do not claim the notebooks reproduce the published paper until the experiment-level results have been checked.

## 6. Figures and interpretation

All experimental images, edge maps, masks and marker overlays used in the manuscript should be exported directly from the verified analysis code. Generative-AI redrawings are **not evidence of the computed segmentation**. For comparisons, use the same source photograph for both methods and include the manual/reference count when available. Label an output of Method 1 as *connected edge components* and an output of Method 2 as *detected foreground markers* unless individual leaf instances have actually been verified.

## 7. Remaining material before this can serve as a complete paper reproduction

- Recover or reconstruct the executable **Balcony and Babyroom crop/rotation/quadrant** preprocessing stage (only a PDF export of that historical notebook was recovered during this audit).
- Deposit a documented photographic dataset, manually recorded counts, corrected counts, correction log and an image-to-record manifest.
- Create a clean, parameterized and **tested** script or notebook for each final method; save exact dependency versions and a deterministic execution log.
- Consolidate the evaluation in one independently checked script; retain historical workbooks as data provenance instead of treating legacy notebook “accuracy” labels as validated metrics.
- Establish which, if any, exploratory PlantCV or OpenCV watershed implementation generated the paper's final counts. If none did, revise the Methods text accordingly.
- Obtain, verify and document the **separate later experiment** and its surface-area/ARIMA/Bayesian code and data from its contributor. Do not mix the two campaigns or infer missing methods from the thesis alone.

## 8. Licensing, credit and citation

**License: to be agreed by all relevant rights holders before public release.** Confirm permission to redistribute both experiments' images, code and collaborators' material. Record the provenance of any externally sourced benchmark images and respect their redistribution terms. Add the final paper citation and the image dataset DOI when available. This repository currently documents a recovered research workflow rather than a certified final software release.
