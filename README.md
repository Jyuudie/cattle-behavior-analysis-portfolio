# cattle-behavior-analysis-portfolio
This repository contains the documentation, results, and presentation for my Capstone Project. The source code and dataset are currently private due to intellectual property agreements with the industry partner (Agriculture Victoria). Access to the codebase can be granted upon request.

![Python](https://img.shields.io/badge/Python-3.12-blue?logo=python&logoColor=white)
![YOLOv8](https://img.shields.io/badge/Model-YOLOv8-green)
![RT-DETR](https://img.shields.io/badge/Model-RT--DETR-orange)
![Status](https://img.shields.io/badge/Status-Capstone%20Complete-success)

# Automated Validation of Methane Sniffer Protocols in Dairy Cattle using Object Dectection

## 📌 Project Overview
**Can we trust the data from methane sensors if we don't know where the cow's head is?**

This project validates the accuracy of  methane sniffer protocols using Computer Vision. By automating the tracking of cow head positions in milking parlors, we correlated behavioral data with sensor logs to biologically validate the standard sampling window.

Using **YOLOv8** and **RT-DETR**, the system tracks cattle under severe occlusion (infrastructure/feed bins) and uses unsupervised K-Means clustering to algorithmically determine feeding thresholds.

## 🚀 Key Features
* **Amodal Detection:** Robust tracking of cattle heads under severe occlusion using custom-trained YOLOv8/RT-DETR models.
* **Algorithmic Thresholding:** Replaced manual guessing with **K-Means Clustering ($k=2$)** to scientifically define "Feeding" vs "Non-Feeding" states.
* **Interactive Tooling:** Built a custom NMS Tuning Tool (`tune_nms.py`) to optimize Confidence/IoU thresholds.
* **Biological Validation:** Confirmed the "Biphasic Feeding Pattern" aligns with the industry-standard 90s–410s sampling window.

## 📂 Repository Structure

| Folder | Description |
| :--- | :--- |
| **`1_preprocessing/`** | Utilities for dataset management, including frame synchronization, negative sample generation, and automated pre-annotation pipelines for active learning. |
| **`2_training/`** | Training loops for YOLOv8/RT-DETR and calibration tools for determining feed rail coordinates. |
| **`3_inference/`** | The core inference engine using ByteTrack and the interactive NMS tuner. |
| **`4_analysis/`** | Scripts for generating the "Herd Feeding Index" and the Biphasic Validation graphs. |
| **`5_utils/`** | Helper scripts for video metadata inspection and frame rate checks. |
| **`docs/`** | Contains the full **Technical Report (PDF)**. |
|**`results/`** | Contains results from Ultralytics training. |

## 🛠️ Tech Stack
* **Computer Vision:** Ultralytics YOLOv8, RT-DETR, OpenCV, ByteTrack
* **Data Analysis:** Pandas, NumPy, Scikit-Learn (K-Means)
* **Visualization:** Matplotlib, Seaborn

## 📊 Results at a Glance
The project successfully processed over **15,000 frames** of milking footage. The analysis revealed a clear "W-shaped" feeding pattern, proving that the standard 320-second sampling window is statistically sufficient to capture representative eructation (burp) events.

![Validation Graph](results/inference_examples/Validation_graph.png)

## 📜 Project Documentation
* **[Slideshow Presentation (PDF)](Docs/Presentation_pdf.pdf):** A 15-slide visual summary of the project goals, methodology, and biological validation results.
* **[Full Technical Report (PDF)](Docs/Final_Report.pdf):** A technical report detailing the methods, error analysis, and statistical validation.

## 📈 Performance & Results

### 1. Model Evaluation (YOLOv8 vs RT-DETR)
The model achieved high precision even in low-light conditions. As seen in the Confusion Matrix, the model successfully distinguishes between the "Head" and background noise.

| Confusion Matrix | 
| :---: |
| ![Confusion Matrix](results/inference_examples/Conf.png) |

### 2. Visual Validation
Below is a sample of the model tracking a cow's head correctly despite severe occlusion from the feed bin and tubing.

![Inference Example](results/inference_examples/Animation3.gif)

### **🔧 Usage**
This project utilizes a **header-based configuration** workflow. Key parameters (file paths, thresholds) are defined in the `=== CONFIGURATION ===` block at the top of each script.

**1. Install Dependencies**
```bash
pip install -r requirements.txt
```
### **📝 License & Citation**
⚠️ **Intellectual Property Notice** This code was developed as part of a Master's Capstone project with DEECA / Agriculture Victoria Research. The intellectual property belong to the host organization. This repository is published for academic and portfolio demonstration purposes only.

Author: Judy Thanh Uyen Nguyen 

Institution: DEECA / Agriculture Victoria Research & La Trobe University

