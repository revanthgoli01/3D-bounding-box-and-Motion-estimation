Here’s a **clean, GitHub-ready `README.md`** for your project, distilled from your report and written in a way that reviewers, recruiters, and classmates will immediately understand.

You can copy-paste this directly into your repository.

---

# 3D Bounding Box Estimation and Motion Prediction using Stereo Vision

Stereo-only 3D vehicle detection and motion estimation pipeline built on the KITTI dataset, using YOLOv8 for 2D detection and a custom ResNet18-based stereo encoder for 3D bounding box regression.

## Overview

This project explores **camera-only 3D perception** as a low-cost alternative to LiDAR for autonomous driving. Given rectified stereo image pairs, the system:

1. Detects cars using a pre-trained YOLOv8 model
2. Estimates depth from stereo disparity
3. Regresses full 3D bounding boxes (center, size, yaw)
4. Tracks vehicles across frames and estimates speed

The pipeline demonstrates that stereo vision alone can recover meaningful 3D structure and motion, while also highlighting challenges such as disparity noise at long range.

---

## Authors

* **Revanth Goli**
* Narsimha Rohit Avadhanula
* Dinesh Sripathi Panditharadhyula
* Sidharth Tipparam

University of Alabama at Birmingham
📧 `{stippara, navadhan, dsripath, golir}@uab.edu`

---

## Key Features

* 🚗 **YOLOv8-based 2D car detection**
* 👀 **Stereo depth estimation** using OpenCV SGBM
* 🧠 **ResNet18 stereo feature encoder**
* 📦 **Multi-task 3D bounding box regression**
* 🧮 **Uncertainty-weighted multi-task loss**
* 🧭 **Yaw, dimensions, and 3D center estimation**
* 🎯 **Simple motion & speed estimation via tracking**
* 📊 Qualitative visualization of 3D boxes and trajectories

---

## Pipeline Architecture

```
KITTI Stereo Images
        |
   YOLOv8 (2D Detection)
        |
   Stereo Cropping
        |
ResNet18 Stereo Encoder
        |
  3D Regression Head
        |
  3D Boxes + Speed Estimation
```

---

## Methodology

### 1. 2D Vehicle Detection

* Uses **YOLOv8s** pre-trained on COCO
* Runs on **left camera images**
* Filters detections to the *car* class
* YOLOv8 tracking mode provides persistent IDs across frames

### 2. Stereo Depth Estimation

* Rectified left–right image pairs
* Disparity computed using **StereoSGBM**
* Depth derived via:

[
Z = \frac{f \cdot B}{d}
]

where:

* `f` = focal length
* `B` = stereo baseline
* `d` = disparity

### 3. Stereo 3D Bounding Box Regression

#### Stereo Feature Encoder

* Backbone: **ResNet18 (ImageNet pretrained)**
* Left & right crops processed independently
* Feature maps concatenated and fused
* Global average pooling → 256-D embedding

#### Regression Head

Predicts:

* **3D center** `(x, y, z)`
* **Dimensions** `(h, w, l)` (enforced positive)
* **Yaw angle** `θ`

Uses a **multi-task uncertainty-weighted loss** to automatically balance tasks.

### 4. Motion & Speed Estimation

* Uses YOLOv8 tracking IDs
* Computes frame-to-frame displacement in 3D
* Speed estimation:

[
v = \frac{\sqrt{\Delta x^2 + \Delta z^2}}{\Delta t}
]

* Converted to km/h for visualization

---

## Dataset

**KITTI Vision Benchmark Suite**

* Rectified stereo images (`image_2`, `image_3`)
* 3D bounding box annotations (`label_2`)
* Camera calibration files
* Classes used: **Car, Van**
* Training subset: ~300 images (for lightweight experimentation)
* No LiDAR data used

---

## Training Details

* Optimizer: **Adam**
* Learning rate: `1e-4`
* Batch size: `4`
* Epochs: `10`
* Backbone: frozen ResNet18
* Trained components: stereo fusion + regression head

### Training Results (300-image subset)

| Epoch | Total Loss | Center L1 (m) | Dims L1 (m) | Yaw L1 (rad) |
| ----- | ---------- | ------------- | ----------- | ------------ |
| 1     | 35.6       | 11.1          | 0.79        | 1.53         |
| 10    | 10.0       | 3.0           | 0.22        | 1.08         |

* Strong improvement in **center and size accuracy**
* Yaw estimation remains noisier (expected with limited data)

---

## Qualitative Results

* Near-range vehicles have **plausible 3D boxes**
* Predicted dimensions closely match real car sizes
* Depth noise increases at long range
* Speed estimates are stable and realistic for urban driving

---

## Limitations

* StereoSGBM disparity is noisy for far objects
* Small training subset, no validation split
* Motion estimation is qualitative only
* Yaw estimation less stable than position/size

---

## Future Work

* Replace SGBM with **RAFT-Stereo or PSMNet**
* Fine-tune YOLOv8 on KITTI
* Add **Kalman filtering** for motion smoothing
* Quantitative evaluation of velocity and acceleration
* Extend to pedestrians and cyclists
* Transformer-based trajectory forecasting (e.g., Trajectron++)

---

## References

* KITTI Vision Benchmark Suite
* YOLOv8 (Ultralytics)
* PSMNet, RAFT-Stereo
* SORT / DeepSORT
* Trajectron++

---

## Project Video

🎥 **Presentation Video:**
(SharePoint link provided in the report)

---

## License

This project is intended for **academic and research use**.

---

If you want, I can also:

* 🗂️ Generate a **recommended repo folder structure**
* 🧾 Add **setup & run commands**
* 🧪 Write an **evaluation script README**
* 🧠 Convert this into a **resume-friendly project summary**

Just tell me what you want next.
