# 🦠 E. coli Instance Segmentation: Geometric & Boundary-Aware Framework

[![Python 3.10](https://img.shields.io/badge/python-3.10-blue.svg)](https://www.python.org/downloads/release/python-3100/)
[![PyTorch 2.0](https://img.shields.io/badge/PyTorch-2.0.1-EE4C2C.svg)](https://pytorch.org/)
[![Detectron2](https://img.shields.io/badge/Detectron2-v0.6-000000.svg)](https://github.com/facebookresearch/detectron2)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

This repository contains the codebase and evaluation framework for the Master's dissertation: **"Instance Segmentation of Deformable Bacterial Cells using Mask R-CNN: A Geometric Shape and Boundary-Aware Evaluation Framework."** It provides an end-to-end deep learning pipeline for segmenting unstained *Escherichia coli* (E. coli) cells in brightfield microscopy, heavily focusing on **Boundary IoU** and **Geometric Shape Analysis** to expose failure modes that standard COCO metrics hide.

---

## 🔬 Overview

Instance segmentation of biological objects in brightfield microscopy is notoriously difficult due to low contrast, irregular morphology, and dense spatial clustering. While standard region-overlap metrics (like mAP and Region IoU) often suggest high performance, they are dominated by interior pixels and mask severe boundary contraction errors at the cell edges.

This project introduces a **4-Phase Pipeline** designed to rigorously evaluate these hidden errors:
1. **Signal Enhancement:** Contrast Limited Adaptive Histogram Equalization (CLAHE) to boost local boundary gradients.
2. **Instance Segmentation:** A ResNet-101-FPN Mask R-CNN architecture trained via Detectron2, with anchor boxes customized for highly elongated rod morphotypes.
3. **Geometric Feature Extraction:** Morphological analysis comparing ground truth vs. predicted Area, Perimeter, Aspect Ratio, Circularity, Eccentricity, and Solidity.
4. **Boundary-Aware Evaluation:** Custom implementation of Boundary IoU alongside spatial crowding/density analysis.

### 🏗️ Pipeline Architecture

![Architecture Diagram](images/architecture%20diagram.jpg) 

---

## 📊 Key Findings & Results

Our multi-metric evaluation on the [DeepBacs E. coli Brightfield dataset](https://zenodo.org/records/5550935) reveals a critical **32.3 percentage point gap** between interior region accuracy and actual boundary delineation.

| Metric | Value | Interpretation |
| :--- | :--- | :--- |
| **mAP (0.50:0.95)** | 59.21% | Strong baseline detection performance for challenging brightfield data. |
| **AP50** | 82.56% | Highly successful at localizing ~82% of all cells. |
| **Mean Region IoU** | 0.790 | Excellent interior pixel classification. |
| **Boundary IoU** | **0.467** | **Exposes severe boundary contraction at the cell edges.** |

### 🧬 Geometric Shape Preservation
Despite boundary contraction causing a **29.4% underestimation in overall cell area**, the model successfully preserves scale-independent biological morphotypes:
* **Eccentricity Error:** Only **0.51%** (Successfully preserves rod-shaped morphology).
* **Solidity Error:** Only **2.88%** (Successfully preserves cell convexity).

### 🔍 Visual Pipeline Results

![Visual Results](images/phase1_to_phase4_visualization.png) 

---

## 🗂️ Repository Structure

```text
.
├── images/
│   ├── architecture diagram.jpg          # Pipeline flowchart
│   └── phase1_to_phase4_visualization.png # 6-panel visual results
├── DeepBacs_E_Coli_Updated.ipynb         # Main Google Colab Notebook (Codebase)
└── README.md                             # Project documentation
