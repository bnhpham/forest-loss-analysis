# Image Processing and Learning for Earth Observation SoSe 2025 Miniproject

This miniproject investigates deforestation in the municipality of São Félix do Xingu, Brazil, from 2018 to 2024 using Sentinel-2 imagery and deep learning. It is subdivided into 3 tasks:

## Task 1: Data Retrieval
* Programmatically queried and retrieved Sentinel-2 imagery directly from the Copernicus Data Space via AWS S3 (`boto3`).
* Used `geopandas` to visualize and verify complete geographic coverage over São Félix do Xingu.

## Task 2: CNN Training for Pixel-Wise Classification
* Finetuned a ResNet18 model pretrained on the BigEarthNet v2 dataset for forest segmentation using PyTorch Lightning.
* Implemented a machine learning (ML) pipeline utilizing custom DataModules and data augmentation techniques.
* Achieved a mean Intersection over Union (mIoU) of 0.745 on a test set.

## Task 3: Bitemporal Change Detection
* Divided the target tile T22MCU into 8,281 individual patches (120x120 pixels at 10m resolution) to calculate absolute ($m^2$) and relative (%) forest loss
* Compared two distinct approaches:
    * **NDVI Differencing:** A physical-index technique using near-infrared and red bands.
    * **CNN Segmentation Differencing:** An ML model utilizing 10 spectral bands.

## Results

| Metric / Aspect | NDVI | CNN |
| :--- | :--- | :--- |
| **Estimated Absolute Loss ($m^2$)** | 536,384,900 $m^2$ | 139,934,100 $m^2$ |
| **Estimated Relative Loss (%)** | 7.29% | 1.80% |
| **Strengths** | • Very efficient, fast and fully explainable. <br>• Requires minimal data input (only 2 spectral bands). | • More robust against atmospheric noise. <br>• Highly flexible; leverages 10 spectral bands to adapt to different biomes and seasons where NDVI fails (e.g. leafless winter forests). |
| **Limitations** | • Sensitive to atmospheric noise causing scattering of non-forest pixels. <br>• Prone to overclassification (e.g. misclassifying meadows or grain fields as forest). <br>• Restricted solely to green vegetation. | • Computationally expensive to train, evaluate and fine-tune. <br>• Depends on quality of training data. <br>• Occasionally failing to recognize forest shapes or missing forested regions entirely. |

## Setup & Installation
### Option 1: pip
To install the required dependencies (GeoPandas, PyTorch etc.), clone this repository and run:
```bash
pip install -r requirements.txt
```

### Option 2: uv
If you have `uv` installed, clone this repository and run:

```bash
# CPU only:
uv sync --python 3.12 --extra cpu

# GPU (CUDA 12.4):
uv sync --python 3.12 --extra cuda124

# GPU (CUDA 11.8):
uv sync --python 3.12 --extra cuda118
```
