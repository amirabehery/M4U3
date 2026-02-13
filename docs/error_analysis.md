# Error Analysis — M4U3 Construction Site Safety Detection (YOLOv8)

This section summarizes the main failure modes observed from the validation outputs (`results/evidence/val_predictions/`) and unseen test predictions (`results/evidence/new_images/`) for the 10-class construction safety model.

## Overall Observations
- The model detects **large, well-lit objects** (Person, Hardhat, Safety Vest, machinery) more reliably than **small or partially occluded objects** (Mask, NO-Mask).
- Errors often come from **PPE status ambiguity** (e.g., face/helmet not visible) rather than completely missing detections.

## Common Failure Modes (What goes wrong)
1. **Small-object misses (Mask / NO-Mask)**
   - Masks occupy very few pixels, especially when people are far away.
   - Side faces, motion blur, and low resolution cause missed detections.

2. **Occlusion and truncation**
   - Hardhats and masks are missed when heads/faces are partially blocked by hands, tools, other workers, or image borders.

3. **Confusion between “Safety Vest” and “NO-Safety Vest”**
   - When clothing colors resemble vests or when the vest is partially visible, the model may flip between the two classes.
   - Reflective stripes and bright clothing can look like vests.

4. **False positives on “NO-*” violation classes**
   - If the head/face/torso is visible but the PPE item is not clearly visible, the model may incorrectly predict a violation (NO-Hardhat / NO-Mask / NO-Safety Vest).

5. **Vehicle vs machinery ambiguity**
   - Some construction vehicles/equipment share similar shapes (e.g., trucks vs loaders), causing occasional confusion between `vehicle` and `machinery`.

6. **Safety Cone confusion in cluttered scenes**
   - Cones are small and sometimes partially hidden; orange objects (barriers, signs, clothing) can trigger false cone detections.

7. **Multiple overlapping people**
   - In crowded scenes, bounding boxes may overlap heavily, causing:
     - duplicate detections for the same person, or
     - one person box covering two people (under-segmentation).

## Class-Level Notes (from metrics + predictions)
- Stronger classes (generally): **Hardhat, Mask, Safety Vest, machinery**
- Weaker classes (generally): **vehicle**, and violation classes **NO-Mask / NO-Hardhat** when visibility is poor  
  (Violations depend on “absence” which is harder to confirm from images.)

## Likely Root Causes
- **Imbalanced data / fewer examples** for some classes (e.g., vehicles, certain violation cases).
- **Label ambiguity**: “NO-*” depends on visibility; if the helmet/face is not visible, the correct label becomes unclear.
- **Resolution limits** at `imgsz=640` for small PPE items.
- **Domain shift**: some test images differ in lighting, camera angle, or background from the training distribution.

## Recommended Improvements (Next Iteration)
1. **Train longer / stronger model**
   - Increase epochs (e.g., 50–100) or try `yolov8s.pt` for better feature capacity.

2. **Increase image size**
   - Try `imgsz=768` or `imgsz=960` to help small objects (masks/helmets), if GPU memory allows.

3. **Class balancing**
   - Add/augment more samples for weak classes (vehicle, NO-Mask, NO-Hardhat).
   - Oversample rare classes or use targeted augmentations.

4. **Refine labels for “NO-*”**
   - Add clearer labeling rules: only label NO-* when the relevant body region is clearly visible.
   - Consider “uncertain” cases or ignore regions if PPE visibility is not verifiable.

5. **Tune inference thresholds**
   - Adjust `conf` and `iou` during prediction to reduce false positives (especially for NO-* classes).

## Evidence Locations in Repo
- Curves + confusion matrices: `results/curves/`
- Ground-truth samples: `results/evidence/annotations/`
- Validation predictions: `results/evidence/val_predictions/`
- Unseen test inference: `results/evidence/new_images/`
