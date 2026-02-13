# M4U3 — Construction Site Safety Detection (YOLOv8)

## Goal
Train and evaluate a YOLOv8 object detection model to detect construction site safety objects and PPE violations from images, and produce both quantitative metrics and visual evidence on unseen test data.

---

## Dataset
Kaggle (Roboflow export):  
https://www.kaggle.com/datasets/snehilsanyal/construction-site-safety-image-dataset-roboflow

The dataset is provided in **YOLO format** with separate **train / validation / test** splits and bounding box annotations.

---

## Classes (10)

0. Hardhat  
1. Mask  
2. NO-Hardhat  
3. NO-Mask  
4. NO-Safety Vest  
5. Person  
6. Safety Cone  
7. Safety Vest  
8. machinery  
9. vehicle  

(Full definitions: `docs/class_definitions.md`)

---

## Repository Structure

- `notebooks/` — training and inference notebooks (Colab-ready)
- `docs/` — class definitions, error analysis, governance checklist
- `results/` — curves and prediction evidence  
  - `results/curves/` — PR/P/R/F1 curves + confusion matrices + `results.png`
  - `results/evidence/annotations/` — labeled (ground-truth) examples
  - `results/evidence/val_predictions/` — validation prediction examples
  - `results/evidence/new_images/` — inference results on unseen/test images
- `weights/` — trained model weights (best.pt / last.pt)

---

## Frameworks and Tools

- Ultralytics YOLOv8
- PyTorch
- OpenCV
- Google Colab (GPU)

---

## Workflow Overview (Steps)

**Step 1 — Environment setup**  
Install and configure YOLOv8 and required libraries.

**Step 2 — Dataset loading and structure check**  
Verify dataset folders and splits (train / valid / test).

**Step 3 — Annotation format validation**  
Inspect label files and class index mapping.

**Step 4 — Dataset configuration (`data.yaml`)**  
Create and verify YOLO dataset configuration file.

**Step 5 — Model training**  
Fine-tune YOLOv8 pretrained weights on the custom dataset.

**Step 6 — Training metrics and curves**  
Analyze loss curves, precision/recall, mAP, and confusion matrices.

**Step 7 — Inference on TEST set**  
Run predictions on unseen test images and save annotated outputs.

**Step 8 — Formal TEST evaluation**  
Compute quantitative metrics on the held-out test split.

**Step 9 — Visual and tabular prediction inspection**  
- Step 9A — Visual bounding box verification  
- Step 9B — Per-image prediction tables

**Step 10 — Export artifacts**  
Package trained weights, metrics, curves, and prediction evidence for submission.

---

## Reproducibility Checklist

- Dataset link: Kaggle — Construction Site Safety Image Dataset (Roboflow)  
  https://www.kaggle.com/datasets/snehilsanyal/construction-site-safety-image-dataset-roboflow
- Train/Val/Test split: Train=2605, Val=114, Test=82 images
- Model variant: YOLOv8n (yolov8n.pt)
- Epochs: 30
- Image size (imgsz): 640
- Batch size: 16
- Ultralytics version: 8.4.14
- Runtime used: Google Colab (GPU — Tesla T4)

## Final Metrics (summary)

**Validation split (114 images)**
- Precision (P): 0.863
- Recall (R): 0.682
- mAP@50: 0.756
- mAP@50–95 ≈ 0.43

**Test split (82 images, unseen)**
- Precision (P): 0.847
- Recall (R): 0.637
- mAP@50: 0.759
- mAP@50–95 ≈ TODO
