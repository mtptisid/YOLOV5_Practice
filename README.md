# YOLO Candy Detection

This repository demonstrates training a YOLOv11s object detection model in Google Colab to identify 11 types of candies in images using the Ultralytics framework.

## Overview

The project involves preparing a custom dataset, splitting it into training and validation sets, training a YOLOv11s model, and testing it on validation images. The trained model achieves an mAP50 of 0.944 and mAP50-95 of 0.86 for detecting candies like MMs, Skittles, and Snickers.

## Prerequisites

- Google account for accessing Google Colab.
- Dataset with images, YOLO-format annotations, and a `classes.txt` file.
- Basic familiarity with Python and object detection.

## Setup

1. Open a new Colab notebook and enable GPU runtime (`Runtime > Change runtime type > GPU`).
2. Install the Ultralytics library:

   ```bash
   !pip install -U ultralytics -q
   ```

## Usage

1. **Prepare Dataset**:

   - Upload your dataset (e.g., `data.zip`) and unzip it to `/content/custom_data`.
   - Run `train_val_split.py` to split images and annotations (90% train, 10% validation):

     ```bash
     !python train_val_split.py --datapath="/content/custom_data" --train_pct=0.9
     ```
   - Create `data.yaml` using the provided Python function to define dataset paths and classes.

2. **Train Model**:

   - Train YOLOv11s for 60 epochs with pretrained weights:

     ```bash
     !yolo detect train data=/content/data.yaml model=yolo11s.pt epochs=60 imgsz=640
     ```
   - Results are saved in `/content/runs/detect/train`.

3. **Test Model**:

   - Run predictions on validation images:

     ```bash
     !yolo detect predict model=runs/detect/train/weights/best.pt source=data/validation/images save=True
     ```
   - Visualize predictions in Colab using IPython to display annotated images.

## Results

- **Dataset**: 162 images (145 train, 17 validation), 11 classes (e.g., MMs_peanut, Skittles).
- **Performance**: mAP50: 0.944, mAP50-95: 0.86.
- **Output**: Trained weights (`best.pt`) and annotated prediction images in `/content/runs/detect/predict`.

