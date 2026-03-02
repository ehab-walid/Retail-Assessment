# 🛒 Retail Shelf Object Detection & Share of Shelf Analytics

This project implements an end-to-end object detection system for automated retail shelf monitoring using **YOLOv8 (Ultralytics)**. All the work for the assessment was done in the **"Retail_Detection_Final.ipynb"** file.

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

After the mentioned changes, running the notebook file **"Retail_Detection_Final.ipynb"** from Google Colab should reproduce the same results.

## Inference

**To run inference, please use "best.pt" model. This is the final baseline model which was chosen.**

The inference script is included in a seperate notebook file "inference.ipynb". This would generate a "run" folder in the content directory of colab with resulted images in the "predict" folder.


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

Dataset Information

- Total Images: 999

- Training: 924 (~92.5%)

- Validation: 40 (~4%)

- Test: 35 (~3.5%)

- Total Classes: 76 SKUs

Dataset format: YOLO format (.txt annotations)

**Key Observations**

- No missing labels

- Noticeable class imbalance

- Small and overlapping products present detection challenges

Exploratory Data Analysis
✔ Verified image-label consistency
✔ Checked dataset splits
✔ Visualized bounding boxes
✔ Analyzed class distribution

Class imbalance was identified as a potential factor affecting recall.

🤖 Model Architecture
Model used: YOLOv8m from Ultralytics

Library:

pip install ultralytics

🚀 Training Strategy
Two models were trained:

1️⃣ Baseline Model
Model: yolov8m.pt

Epochs: 100

Image size: 640

Seed: 7

model = YOLO(“yolov8m.pt”)
model.train(data=“data.yaml”, epochs=100, imgsz=640)

2️⃣ High-Resolution Model
Model: yolov8m.pt

Epochs: 150

Image size: 1024

Early stopping patience: 50

Training stopped at epoch 67

model = YOLO(“yolov8m.pt”)
model.train(data=“data.yaml”, epochs=150, imgsz=1024, patience=50)

📊 Evaluation Strategy
Models were evaluated on the test set with varying confidence thresholds:

- 0.25

- 0.20

- 0.15

IoU Threshold: 0.6

📈 Results Summary
🔹 Baseline Model
| Confidence | Recall | Precision | mAP@0.5 |
| ---------- | ------ | --------- | ------- |
| 0.25       | 0.777  | 0.757     | 0.811   |
| 0.20       | 0.805  | 0.744     | 0.822   |
| 0.15       | 0.805  | 0.738     | 0.822   |

✔ Lowering confidence improved Recall
✔ Precision dropped slightly but remained acceptable

🔹 High-Resolution Model (1024)
| Confidence | Recall | Precision | mAP@0.5 |
| ---------- | ------ | --------- | ------- |
| 0.25       | 0.800  | 0.729     | 0.814   |
| 0.20       | 0.798  | 0.711     | 0.814   |
| 0.15       | 0.805  | 0.705     | 0.821   |


✔ Higher resolution helped detect small products
✔ Slight precision tradeoff observed

🏆 Final Outcome
Best trade-off achieved at:

Confidence = 0.20 (Baseline Model)

Recall ≈ 80.5%

Precision ≈ 73.8%

mAP@0.5 ≈ 82.2%

This significantly improves recall compared to the original 67.6%.

📦 Share of Shelf Analysis
The model was also used to:

Count product instances

Calculate product percentage distribution

Visualize dominance using bar plots

This enables automated shelf analytics.
