# SE4050 - Deep Learning Assignment
## Multi-Class Brain Tumor MRI Classification

**Module**: SE4050 - Deep Learning  
**Degree**: BSc (Hons) in Information Technology  
**Institution**: Sri Lanka Institute of Information Technology (SLIIT)  
**Academic Year**: 2026  

---

## Group Information

| Student ID | Student Name | Assigned Component |
|------------|--------------|-------------------|
| ITxxxxxxxx | Student 1    | `01_EDA.ipynb` & `03_CNN.ipynb` |
| ITxxxxxxxx | Student 2    | `02_Preprocessing.ipynb` & `04_VGG16.ipynb` |
| ITxxxxxxxx | Student 3    | `05_ResNet50.ipynb` |
| ITxxxxxxxx | Student 4    | `06_EfficientNetB3.ipynb` & `07_Model_Comparison.ipynb` |

---

## Project Overview

This project implements and compares four supervised deep learning architectures to classify brain MRI scans into four diagnostic categories:
- **Glioma Tumor**
- **Meningioma Tumor**
- **Pituitary Tumor**
- **No Tumor** (healthy control)

The objective is to evaluate a baseline Convolutional Neural Network trained from scratch against three pretrained transfer learning architectures (VGG16, ResNet50, EfficientNetB3) using standardized preprocessing and evaluation metrics.

---

## Models Implemented

| Model | Type | Pretrained Source | Input Size | Key Architecture Features |
|---|---|---|---|---|
| **Custom CNN** | From Scratch | None (Random Init) | 224 x 224 x 3 | 4 Conv-BN-ReLU-Pool blocks, Global Average Pooling, Dropout (0.4) |
| **VGG16** | Transfer Learning | ImageNet | 224 x 224 x 3 | Frozen feature extractor + Fine-tuning of top conv blocks (block4/5) |
| **ResNet50** | Transfer Learning | ImageNet | 224 x 224 x 3 | Residual bottleneck connections + Fine-tuning of layer4 |
| **EfficientNetB3** | Transfer Learning | ImageNet | 224 x 224 x 3 | MBConv inverted residual blocks with Squeeze-and-Excitation attention |

---

## Dataset & Partitioning

- **Source**: [Brain Tumor MRI Dataset (Kaggle)](https://www.kaggle.com/datasets/masoudnickparvar/brain-tumor-mri-dataset) by Masoud Nickparvar (2021).
- **Total Images**: 3,264 axial MRI scans across 4 classes.
- **Data Splitting**: A single, unified stratified split of **70% Train / 15% Validation / 15% Test** applied in `02_Preprocessing.ipynb` with `seed=42`.

| Class | Total Images | Training (70%) | Validation (15%) | Test (15%) | Class Weight |
|---|---:|---:|---:|---:|---:|
| Glioma Tumor | 926 | 648 | 139 | 139 | 0.8812 |
| Meningioma Tumor | 937 | 656 | 140 | 141 | 0.8709 |
| No Tumor | 500 | 350 | 75 | 75 | 1.6320 |
| Pituitary Tumor | 901 | 631 | 135 | 135 | 0.9053 |
| **Total** | **3,264** | **2,285** | **489** | **490** | **Mean = 1.0000** |

*Note: Class weights are computed using the balanced inverse-frequency formula to prevent bias toward the majority classes.*

---

## Repository Structure

```
SE4050--Deep-Learning-Assignment_Final/
|-- 01_EDA.ipynb                 # Step 1: Exploratory Data Analysis
|-- 02_Preprocessing.ipynb       # Step 2: Unified 70/15/15 preprocessing & data split
|-- 03_CNN.ipynb                 # Step 3: Custom CNN baseline
|-- 04_VGG16.ipynb               # Step 4: VGG16 Transfer Learning
|-- 05_ResNet50.ipynb            # Step 5: ResNet50 Transfer Learning
|-- 06_EfficientNetB3.ipynb      # Step 6: EfficientNetB3 Transfer Learning
|-- 07_Model_Comparison.ipynb    # Step 7: Cross-model performance comparison
|
|-- Dataset/                     # Downloaded raw dataset from Kaggle
|   |-- README.md                # Download & extraction instructions
|   |-- Training/                # Raw training images per class
|   `-- Testing/                 # Raw testing images per class
|
|-- preprocessed_data/           # Output from 02_Preprocessing.ipynb (shared .npy arrays)
|   |-- X_train.npy, y_train.npy
|   |-- X_val.npy, y_val.npy
|   |-- X_test.npy, y_test.npy
|   |-- class_weights.npy
|   `-- preprocessing_config.json
|
|-- saved_models/                # Best model checkpoints (.pt)
|   |-- CNN/
|   |-- VGG16/
|   |-- ResNet50/
|   `-- EfficientNetB3/
|
|-- results/                     # Metric JSON files and evaluation plots
|   |-- *_metrics.json
|   |-- master_results.json
|   `-- *.png
|
|-- requirements.txt             # Python dependencies
|-- .gitignore                   # Excludes raw images, large .npy arrays, and .pt weights
`-- README.md                    # Project documentation
```

---

## Installation & Setup

### Option 1: Local Machine Setup

```bash
# 1. Clone the repository
git clone https://github.com/Kavinigamalath/SE4050--Deep-Learning-Assignment_Final.git
cd SE4050--Deep-Learning-Assignment_Final

# 2. Create and activate a virtual environment
python -m venv .venv

# On Windows:
.\.venv\Scripts\activate
# On Linux/macOS:
source .venv/bin/activate

# 3. Install dependencies
pip install -r requirements.txt

# 4. (Optional) For NVIDIA GPU acceleration on Windows/Linux:
pip install torch torchvision --index-url https://download.pytorch.org/whl/cu128

# 5. Launch JupyterLab
jupyter lab
```

### Option 2: Google Colab Setup (Free T4 GPU)

1. Open [Google Colab](https://colab.research.google.com) and set runtime: **Runtime** -> **Change runtime type** -> **T4 GPU**.
2. Run in a code cell:
   ```python
   !git clone https://github.com/Kavinigamalath/SE4050--Deep-Learning-Assignment_Final.git
   %cd SE4050--Deep-Learning-Assignment_Final
   !pip install -q -r requirements.txt
   ```

---

## Dataset Download

The raw dataset images are excluded from Git to keep the repository lightweight (< 5 MB).

1. Download `archive.zip` from [Kaggle: Brain Tumor MRI Dataset](https://www.kaggle.com/datasets/masoudnickparvar/brain-tumor-mri-dataset).
2. Extract the archive into the `Dataset/` folder so that `Dataset/Training/` and `Dataset/Testing/` are populated.

Or download via Kaggle CLI:
```bash
kaggle datasets download -d masoudnickparvar/brain-tumor-mri-dataset
unzip -q brain-tumor-mri-dataset.zip -d Dataset/
```

---

## Execution Order

The notebooks must be executed sequentially:

1. **`01_EDA.ipynb`**: Analyzes class distributions, image dimensions, intensity profiles, and mean images.
2. **`02_Preprocessing.ipynb`**: **Required once.** Resizes images to 224x224, normalizes to [0, 1], performs the 70/15/15 stratified split, computes class weights, and saves arrays to `preprocessed_data/`.
3. **`03_CNN.ipynb`**: Trains the custom 4-block CNN baseline from scratch.
4. **`04_VGG16.ipynb`**: Trains VGG16 with feature extraction (Phase 1) and fine-tuning (Phase 2).
5. **`05_ResNet50.ipynb`**: Trains ResNet50 with feature extraction (Phase 1) and residual bottleneck fine-tuning (Phase 2).
6. **`06_EfficientNetB3.ipynb`**: Trains EfficientNetB3 with compound scaling and fine-tuning.
7. **`07_Model_Comparison.ipynb`**: Aggregates all model metrics, generates comparative tables, ROC-AUC curves, confusion matrices, and summarizes results.

---

## Evaluation Metrics

All models are evaluated on the identical 490-image test set using:
- **Test Accuracy & Balanced Accuracy**
- **Precision, Recall, and F1-Score** (Macro and Weighted averages)
- **One-vs-Rest (OvR) ROC-AUC** per class
- **Confusion Matrix** (raw counts and normalized)

### Summary of Results (Test Set)

| Model | Test Accuracy | Macro Precision | Macro Recall | Macro F1-Score | Weighted ROC-AUC |
|---|---|---|---|---|---|
| **Custom CNN** | *Evaluated in 03* | *Evaluated in 03* | *Evaluated in 03* | *Evaluated in 03* | *Evaluated in 03* |
| **VGG16** | *Evaluated in 04* | *Evaluated in 04* | *Evaluated in 04* | *Evaluated in 04* | *Evaluated in 04* |
| **ResNet50** | 87.76% | 87.18% | 88.95% | 87.84% | 0.9788 |
| **EfficientNetB3** | *Evaluated in 06* | *Evaluated in 06* | *Evaluated in 06* | *Evaluated in 06* | *Evaluated in 06* |

*Complete metrics, ROC curves, and cross-model comparison charts are generated in `07_Model_Comparison.ipynb`.*

---

## References

1. Simonyan, K., & Zisserman, A. (2015). Very deep convolutional networks for large-scale image recognition. *ICLR*.
2. He, K., Zhang, X., Ren, S., & Sun, J. (2016). Deep residual learning for image recognition. *CVPR*.
3. Tan, M., & Le, Q. V. (2019). EfficientNet: Rethinking model scaling for convolutional neural networks. *ICML*.
4. Nickparvar, M. (2021). Brain Tumor MRI Dataset. *Kaggle*. https://www.kaggle.com/datasets/masoudnickparvar/brain-tumor-mri-dataset
