# M4U3 — Construction Site Safety Detection (YOLOv8)

## AECO problem framing (short)
Construction sites require continuous monitoring of PPE compliance and safety-related hazards. Manual inspection is slow and inconsistent. This project trains a YOLOv8 object detection model to detect common construction safety objects and PPE violations in images.

## Success criteria (short)
- Reproducible **cloud-only** workflow: a third party can open this repo in a browser and run the notebook in **Google Colab** (no local installs).
- Reports metrics (**Precision, Recall, mAP@50, mAP@50–95**) and exports curves/confusion matrices.
- Evidence pack includes:
  - 3–5 annotation examples
  - validation predictions (≥10 examples; provided as batch-collage screenshots)
  - more than 5 new/unseen-image prediction examples
- Includes short error analysis and governance/licensing notes.

---

## Dataset
Kaggle (Roboflow export):  
https://www.kaggle.com/datasets/snehilsanyal/construction-site-safety-image-dataset-roboflow

**Format:** YOLO (images + labels) with train/valid/test splits.

**Split used (dataset-provided):**
- Train: 2605 images
- Val: 114 images
- Test: 82 images

> Note: The dataset export includes train/valid/test splits (not a strict 80/20-only split). This repo uses the dataset-provided splits and reports metrics on validation and on the held-out test set.

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

Label rules and definitions: `docs/class_definitions.md`

---

## Results summary (metrics + takeaways)

### Validation (114 images)
- Precision (P): 0.863
- Recall (R): 0.682
- mAP@50: 0.756
- mAP@50–95: ~0.435

### Test (82 images, unseen)
- Precision (P): 0.847
- Recall (R): 0.637
- mAP@50: 0.759
- mAP@50–95: TODO

**Key takeaways (2–3):**
- Stronger performance on large/clear objects (Person, Hardhat, Safety Vest, machinery) than on small/ambiguous PPE signals (Mask/NO-Mask).
- Violation classes (`NO-*`) are harder because “absence of PPE” is ambiguous when the relevant body region is occluded or low-resolution.
- Some confusion occurs between `vehicle` and `machinery` due to visual similarity across construction equipment.

Curves + confusion matrices: `results/curves/`  
Evidence pack: `results/evidence/`

---

## How to reproduce (Colab steps — copy/paste)

### 1) Open notebook in Colab
Open this notebook from the repo in Google Colab:
- `notebooks/M4U3_Train_yolov8.ipynb`

### 2) Run all cells
In Colab:
- Runtime → **Restart runtime**
- Runtime → **Run all**

Expected outputs:
- Training run outputs (or loading trained weights if training is skipped)
- Saved curves and confusion matrices
- Inference outputs on the test/unseen split saved as annotated images
- Quantitative evaluation on the held-out test split

---

## Reproducibility Checklist (final run)
- Dataset link: Kaggle — Construction Site Safety Image Dataset (Roboflow)  
  https://www.kaggle.com/datasets/snehilsanyal/construction-site-safety-image-dataset-roboflow
- Dataset version: Kaggle snapshot (downloaded on 2026-02-13)
- Train/Val/Test split: Train=2605, Val=114, Test=82 images
- Model variant: YOLOv8n (yolov8n.pt)
- Epochs: 30
- Image size (imgsz): 640
- Batch size: 16
- Ultralytics version: 8.4.14
- Runtime used: Google Colab (GPU — Tesla T4)

---

## Reproducibility proof note (required)
- Last successful run: 2026-02-13 (Colab)
- Hardware: Tesla T4 GPU
- Expected runtime: ~20–30 minutes total (dataset extraction + 30-epoch training + evaluation), depending on Colab load

---

## Repo structure
- `notebooks/` — training + evaluation notebook (Colab-ready)
- `docs/` — class definitions, error analysis, governance checklist
- `results/` — curves and prediction evidence
- `weights/` — trained weights (`best.pt`, `last.pt`)

---

## Evidence locations (rubric mapping)
- Curves + confusion matrices: `results/curves/`
- Annotation examples (3–5 screenshots): `results/evidence/annotations/`
- New/unseen images predictions (5 screenshots): `results/evidence/new_images/`

### Validation predictions (rubric: 10)
Validation predictions are provided as **3 YOLO batch-collage screenshots**:
- `results/evidence/val_predictions/val_batch0_pred.jpg`
- `results/evidence/val_predictions/val_batch1_pred.jpg`
- `results/evidence/val_predictions/val_batch2_pred.jpg`

Each batch image is a collage containing **~16 validation images**, so the repo includes **~48 validation prediction examples total** (exceeding the “10 validation predictions” requirement).

---

## Docs
- Class definitions: `docs/class_definitions.md`
- Error analysis: `docs/error_analysis.md`
- Governance checklist: `docs/governance_checklist.md`

---

## Governance + licensing
- Governance checklist: `docs/governance_checklist.md`
- Repository license: see `LICENSE`
- Dataset rights/terms: dataset is sourced from Kaggle and is subject to Kaggle/dataset terms and licensing.
**Test split (82 images, unseen)**
- Precision (P): 0.847
- Recall (R): 0.637
- mAP@50: 0.759
- mAP@50–95 ≈ TODO
