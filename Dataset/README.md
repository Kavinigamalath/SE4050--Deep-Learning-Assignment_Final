# Dataset Download Instructions

The raw dataset images are excluded from this GitHub repository to maintain a lightweight codebase and adhere to Kaggle dataset distribution best practices.

## Dataset Source
- **Source**: [Brain Tumor MRI Dataset on Kaggle](https://www.kaggle.com/datasets/masoudnickparvar/brain-tumor-mri-dataset)
- **Author**: Masoud Nickparvar (2021)
- **Total Images**: 3,264 axial MRI scans (4 classes)

---

## Option 1: Manual Download via Browser (Easiest)

1. Go to the Kaggle dataset page:
   https://www.kaggle.com/datasets/masoudnickparvar/brain-tumor-mri-dataset
2. Click **Download** (downloads `archive.zip` ~88 MB).
3. Extract the contents directly into this `Dataset/` directory.
4. Verify the folder structure looks like this:
   ```
   Dataset/
   |-- README.md
   |-- Training/
   |   |-- glioma_tumor/
   |   |-- meningioma_tumor/
   |   |-- no_tumor/
   |   `-- pituitary_tumor/
   `-- Testing/
       |-- glioma_tumor/
       |-- meningioma_tumor/
       |-- no_tumor/
       `-- pituitary_tumor/
   ```

---

## Option 2: Automated Download via Kaggle CLI

If you have the Kaggle CLI configured (`~/.kaggle/kaggle.json`):

```bash
# 1. Install kaggle CLI if not already installed
pip install kaggle

# 2. Download dataset archive
kaggle datasets download -d masoudnickparvar/brain-tumor-mri-dataset

# 3. Unzip into the Dataset directory
# On Linux/macOS/Colab:
unzip -q brain-tumor-mri-dataset.zip -d Dataset/

# On Windows (PowerShell):
Expand-Archive -Path brain-tumor-mri-dataset.zip -DestinationPath Dataset/
```

---

## Option 3: In Google Colab

Run this code cell in Colab:
```python
import os

# Create Dataset directory if not exists
os.makedirs('Dataset', exist_ok=True)

# Download and extract using opendatasets or kaggle API
!pip install -q opendatasets
import opendatasets as od
od.download('https://www.kaggle.com/datasets/masoudnickparvar/brain-tumor-mri-dataset', data_dir='Dataset')
```
