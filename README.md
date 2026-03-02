# 🛒 Retail Shelf Object Detection & Share of Shelf Analytics

This project implements an end-to-end object detection system for automated retail shelf monitoring using **YOLOv8 (Ultralytics)**.

---

## 🎯 Project Objectives

- ✅ Improve **Recall** to reduce missed shelf items  
- ✅ Maintain strong **Precision** to avoid false stock counts  
- ✅ Treat the entire test dataset as a representative shelf  
- ✅ Compute **Percentage Share of Shelf** per SKU  

---

## Approach

The model detects 76 retail SKUs from shelf images and evaluates performance using:

- Precision  
- Recall  
- mAP@50  
- mAP@50-95  

After evaluation, the entire test dataset is treated as a single shelf, and SKU distribution is calculated as:

Share of Shelf (%) = (Detected SKU Count / Total Detected Products) × 100


This provides retail-style analytics similar to real store monitoring systems.

---

## Setup Instructions

### Environment

All experiments were conducted using **Google Colab** with GPU acceleration.

---

### Mount Google Drive

Inside Colab:

```python
from google.colab import drive
drive.mount('/content/drive')
```

### Dataset Structure

The dataset must be placed inside Google Drive with the following structure:

MyDrive/
- │
- ├── dataset/
- │   ├── train/
- │   ├── valid/
- │   ├── test/
- │   └── data.yaml

### Create Training Logs Folder

Inside MyDrive, create a folder named:

Retail Assessment

This folder is used to copy and store:

- Training logs

- Model checkpoints

- Validation results

- The notebook includes a script that automatically copies training outputs into this directory.
