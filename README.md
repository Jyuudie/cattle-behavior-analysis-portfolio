# Automated Validation of Methane Sniffer Protocols in Dairy Cattle using Object Detection

[![Python](https://img.shields.io/badge/Python-3.12-blue?logo=python&logoColor=white)](https://www.python.org/)
[![YOLOv8](https://img.shields.io/badge/Model-YOLOv8-green)](https://github.com/ultralytics/ultralytics)
[![RT-DETR](https://img.shields.io/badge/Model-RT--DETR-orange)](https://github.com/lyuwenyu/RT-DETR)
![Status](https://img.shields.io/badge/Status-Capstone%20Complete-success)

> **Note:** This repository contains the documentation, results, and presentation for my Master's Capstone Project. The source code and dataset are currently private due to intellectual property agreements with the industry partner (**Agriculture Victoria**). Access to the codebase can be granted upon request (see [Contact](#-contact--access)).

---

## 📌 Project Overview
**Can we trust the data from methane sensors if we don't know where the cow's head is?**

This project validates the accuracy of methane sniffer protocols using **Computer Vision**. By automating the tracking of cow head positions in milking parlors, we correlated behavioral data with sensor logs to biologically validate the standard sampling window.

Using **YOLOv8** and **RT-DETR**, the system tracks cattle under severe occlusion (infrastructure/feed bins) and uses unsupervised **K-Means clustering** to algorithmically determine feeding thresholds.

## 🚀 Key Features
* **Amodal Detection:** Robust tracking of cattle heads under severe occlusion (feed bins/rails).
* **Algorithmic Thresholding:** Replaced manual guessing with **K-Means Clustering ($k=2$)** to scientifically define "Feeding" vs "Non-Feeding" states.
* **Active Learning Pipeline:** Implemented an auto-labeling loop (Script A.10) to rapidly scale the dataset using model-assisted annotation.
* **Biological Validation:** Confirmed that the herd's "Biphasic Feeding Pattern" aligns with the industry-standard **90s–410s sampling window**.

## 🏗️ System Architecture & Pipeline
The project is structured into **12 modular scripts** that handle the full lifecycle from raw video ingestion to statistical validation.

### Phase 1: Data Preparation & Active Learning
| ID | Script | Functionality |
| :--- | :--- | :--- |
| **A.9** | `extract_inference_frames.py` | **Data Ingestion:** Converts raw video footage into static image sequences (5fps) to prepare for the auto-labeling pipeline. |
| **A.10** | `generate_cvat_preannotations.py` | **Active Learning:** Uses the trained model to predict bounding boxes on new frames, creating pre-annotated text files for rapid correction in CVAT. |
| **A.1** | `sync_frames.py` | **Data Synchronization:** Iterates through CVAT exports, matches them to raw video frames, and extracts high-res image-text pairs for YOLO/COCO formatting. |

### Phase 2: Calibration & Training
| ID | Script | Functionality |
| :--- | :--- | :--- |
| **A.2** | `find_threshold.py` | **Spatial Calibration:** A GUI utility to manually identify the "Feeding Horizon" (pixel y-coordinate) of the feed rail for specific camera angles. |
| **A.3** | `train_models.py` | **Training Loop:** Configures and initiates training for **YOLOv8** and **RT-DETR**, including custom augmentations (Mosaic, MixUp) and hyperparameter definitions. |

### Phase 3: Inference & Tracking
| ID | Script | Functionality |
| :--- | :--- | :--- |
| **A.5** | `extract_tracks.py` | **Batch Inference:** Applies **ByteTrack** to raw video footage, extracting temporal coordinate data (x,y) for every identified animal into CSV format. |
| **A.11** | `run_rtdetr_inference.py` | **Transformer Inference:** Dedicated execution script for the **RT-DETR** architecture, loading Vision Transformer weights with attention-specific parameters. |
| **A.4** | `visualize_inference.py` | **Qualitative Verification:** Runs the model with `stream=True` to generate annotated video outputs for visual verification of tracking stability. |

### Phase 4: Analysis & Validation
| ID | Script | Functionality |
| :--- | :--- | :--- |
| **A.6** | `aggregate_sessions.py` | **Data Merging:** Computes the spatial mean (average y-position) of the herd per frame and merges all sessions into a `Master_Herd_Feeding_Index.csv`. |
| **A.7** | `plot_herd_index.py` | **Single-Session Analysis:** Applies a rolling average (Window=30) to smooth sensor noise and plot head position against the calibrated threshold. |
| **A.12** | `plot_bimodal_distribution.py` | **Statistical Validation:** Generates a frequency histogram of head positions. The resulting **Bimodal Distribution** statistically confirms two discrete behavioral states (Feeding vs. Idling). |
| **A.8** | `plot_validation_summary.py` | **Biphasic Validation:** The final analytical script. Applies Min-Max Scaling and **K-Means Clustering** to generate the "Spaghetti Plot," proving biological alignment with sensor data. |

## 📊 Results at a Glance
The project successfully processed over **15,000 frames** of milking footage. The analysis revealed a clear "W-shaped" feeding pattern, proving that the standard sampling window is statistically sufficient.

![Validation Graph](Docs/Validation_graph.png)
*(Figure: The "W-Shape" Feeding Pattern detected by the Computer Vision system, aligned with the methane sampling window.)*

## 📜 Project Documentation
* 📄 **[Slideshow Presentation (PDF)](Docs/Presentation_pdf.pdf)**
    * *A 15-slide visual summary of goals, methodology, and biological validation results.*
* 📄 **[Full Technical Report (PDF)](Docs/FINAL_REPORT.pdf)**
    * *A detailed report covering error analysis, "failure modes," and statistical validation.*

## 📈 Performance & Evaluation
### 1. Model Benchmarking (YOLOv8 vs RT-DETR)
The model achieved high precision even in low-light conditions. As seen in the Confusion Matrix, the model successfully distinguishes between the "Head" and background noise.

| Confusion Matrix |
| :---: |
| ![Confusion Matrix](Docs/conf.png) |

### 2. Visual Validation
Below is a sample of the model tracking a cow's head correctly despite severe occlusion from the feed bin and tubing.

![Inference Example](Docs/Animation3.gif)

## 📝 License & Citation
⚠️ **Intellectual Property Notice**
This code was developed as part of a Master's Capstone project with **DEECA / Agriculture Victoria Research**. The intellectual property, dataset, and source code belong to the host organization. This repository is published for academic and portfolio demonstration purposes only.

### 📧 Contact & Access
**Judy Thanh Uyen Nguyen**
* **Institution:** DEECA / Agriculture Victoria Research & La Trobe University
* **LinkedIn:** [Your LinkedIn Profile URL Here]
* **Email:** [judy.n88@live.com.au]
