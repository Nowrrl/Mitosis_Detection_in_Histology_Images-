# Mitosis Detection in Histology Images Using Bag of Visual Words and SVM

## Project Overview
Breast cancer severity assessment often relies on counting mitotic figures in histopathological images.  
This project implements a pipeline to detect mitosis using **Bag of Visual Words (BoVW)** and **Support Vector Machine (SVM)**.  
The pipeline follows four stages:
1. **Visualization of Mitosis Coordinates**
2. **Preprocessing of Images**
3. **Feature Extraction using BoVW**
4. **Classification with SVM**

---

## Methodology

### 1. Visualization
- Load histology images in BMP format (2084×2084 resolution).  
- Overlay mitosis coordinates from CSV files to visually verify their locations.  
- Visualize the detected mitotic figures to ensure data correctness.  

---

### 2. Preprocessing
- Convert images to grayscale.  
- Apply **Otsu’s thresholding** (inverted) to separate foreground cells from the background.  
- Use **connected-components analysis** to identify potential cell or nucleus bounding boxes.  

---

### 3. Feature Extraction (BoVW)
- Extract **SIFT descriptors** from cell patches (limited to 50 descriptors per patch).  
- Build a **visual vocabulary** using **MiniBatchKMeans** clustering (K=100).  
- Represent each bounding box with a **normalized histogram of word occurrences**.  
- Generate BoVW histograms for all bounding boxes for training.  

---

### 4. Classification (SVM)
- Label bounding boxes as **mitosis** or **non-mitosis** based on overlap with CSV coordinates.  
- Train an **SVM** with RBF or linear kernel using the BoVW histograms.  
- Handle class imbalance with:
  - **Oversampling of minority (mitosis) class**  
  - **F1-based GridSearchCV** for hyperparameter optimization.  
- Predict on the **A00 (test) dataset** and compute IoU with ground truth coordinates.  

---

## Results

| Dataset          | F1 (Mitosis) | Mean IoU |
|-----------------|-------------|---------|
| Validation (A02) | 0.06        | -       |
| Test (A00)       | -           | 0.0     |

- High overall accuracy (>98%) on non-mitotic cell detection.  
- Low F1 for the positive (mitosis) class, indicating limited effectiveness due to severe class imbalance.  
- The final mean IoU on the test set (A00) is 0.0, showing difficulty in accurately detecting mitotic cells.  

---

## Challenges
- **Class Imbalance:** Mitosis cases are significantly fewer than non-mitosis cases.  
- **Feature Extraction Complexity:** Selecting relevant features from noisy histology images.  
- **Performance Metrics:** High accuracy does not equate to good detection for the positive class.
