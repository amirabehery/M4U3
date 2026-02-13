# Model Governance Checklist — Construction Site Safety Detection (YOLOv8)

This checklist documents basic governance considerations for the M4U3 construction site safety object detection model.

## 1) Purpose and Intended Use
- **Intended use:** Assist with monitoring construction-site safety by detecting PPE items and potential PPE violations in images (e.g., hardhat/vest/mask presence).
- **Target outputs:** Bounding boxes + class labels for: Hardhat, Mask, NO-Hardhat, NO-Mask, NO-Safety Vest, Person, Safety Cone, Safety Vest, machinery, vehicle.
- **Users:** Students / researchers; potentially safety teams as a *decision-support* tool.
- **Not intended for:** Automated enforcement or disciplinary action without human review.

## 2) Data Source and Licensing
- **Dataset source:** Kaggle (Roboflow export) — Construction Site Safety Image Dataset  
  https://www.kaggle.com/datasets/snehilsanyal/construction-site-safety-image-dataset-roboflow
- **Data type:** Images with bounding box annotations in YOLO format.
- **License/terms:** Follow Kaggle dataset terms and any Roboflow export terms.  
  (If required, record the dataset license name from Kaggle here.)

## 3) Privacy and Personal Data
- **Personal data present:** Images contain people; faces may be visible.
- **Privacy risk:** The model can highlight people in images (Person class) and potentially enable surveillance-like use.
- **Mitigations:**
  - Use only for the assignment / academic purposes.
  - Avoid publishing any sensitive or identifying images beyond what is required as evidence.
  - If deploying, consider face blurring and access control for stored images/predictions.

## 4) Safety, Ethics, and Human Oversight
- **Human-in-the-loop:** Required. Predictions must be reviewed by a human, especially for violation classes (`NO-*`).
- **Risk of harm:** False positives may incorrectly flag workers; false negatives may miss unsafe situations.
- **Mitigations:**
  - Display confidence scores and require manual confirmation.
  - Use the model as an *aid*, not as a final authority.

## 5) Model Limitations (Known)
- **Visibility dependence:** `NO-*` classes are ambiguous if the head/face/torso is occluded or too small.
- **Small object sensitivity:** Mask/NO-Mask performance degrades when people are far from the camera or image quality is low.
- **Domain shift:** Performance may drop on new sites, different PPE styles, lighting, camera angles, weather, or image resolution.
- **Class confusion:** Safety Vest vs NO-Safety Vest and vehicle vs machinery may confuse in some scenes.

## 6) Bias and Fairness Considerations
- **Potential bias sources:** Dataset may over-represent certain environments, PPE colors/styles, camera viewpoints, and worker appearance.
- **Impact:** Uneven performance across different sites or conditions could lead to inconsistent safety flagging.
- **Mitigations:**
  - Evaluate across diverse scenes and conditions.
  - Expand training data to include varied sites, PPE types, lighting, and viewpoints.

## 7) Security and Misuse
- **Misuse risk:** Could be used for increased surveillance or monitoring without consent.
- **Mitigations:**
  - Restrict access to model weights and prediction outputs if used outside coursework.
  - Document intended use and prohibit automated punitive decisions.

## 8) Reproducibility and Traceability
- **Training artifacts stored:** curves, confusion matrices, evidence images, weights (`best.pt`, `last.pt`) in the repository.
- **Notebook available:** training + validation + test inference notebook in `notebooks/`.
- **Key settings recorded:** model variant, epochs, imgsz, batch size, Ultralytics version (see `README.md`).

## 9) Monitoring and Maintenance (If Deployed)
- **Monitoring:** Track accuracy drift over time and across sites; periodically review false positives/negatives.
- **Updates:** Retrain or fine-tune when PPE styles, camera setup, or environment changes significantly.
- **Retesting:** Re-run evaluation on a held-out test set after any update.

## 10) Accountability
- **Owner:** Project student (course assignment).
- **Decision responsibility:** Human reviewer/operator; the model provides suggestions only.
