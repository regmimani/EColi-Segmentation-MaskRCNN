# E. coli Instance Segmentation: Geometric & Boundary-Aware Framework

[![Python 3.10](https://img.shields.io/badge/python-3.10-blue.svg)](https://www.python.org/downloads/release/python-3100/)
[![PyTorch 2.0](https://img.shields.io/badge/PyTorch-2.0.1-EE4C2C.svg)](https://pytorch.org/)
[![Detectron2](https://img.shields.io/badge/Detectron2-v0.6-000000.svg)](https://github.com/facebookresearch/detectron2)

This repository contains the code and evaluation framework for the dissertation: **"Instance Segmentation of Deformable Bacterial Cells using Mask R-CNN: A Geometric Shape and Boundary-Aware Evaluation Framework."** It provides an end-to-end deep learning pipeline for segmenting *Escherichia coli* (E. coli) cells in brightfield microscopy, heavily focusing on **Boundary IoU** and **Geometric Shape Analysis** to expose failure modes hidden by standard COCO metrics.

---

## 🔬 Overview

Instance segmentation of unstained bacterial cells in brightfield microscopy is notoriously difficult due to low contrast, irregular morphology, and dense spatial clustering. While standard region-overlap metrics (like standard mAP) suggest high performance, they often mask severe boundary contraction errors.

This project introduces a **4-Phase Pipeline**:
1. **Signal Enhancement:** Contrast Limited Adaptive Histogram Equalization (CLAHE) to boost local boundary gradients.
2. **Instance Segmentation:** A ResNet-101-FPN Mask R-CNN architecture trained via Detectron2.
3. **Geometric Feature Extraction:** Morphological analysis (Area, Perimeter, Aspect Ratio, Circularity, Eccentricity, Solidity).
4. **Boundary-Aware Evaluation:** Custom implementation of Boundary IoU and spatial crowding/density analysis.

![Architecture Diagram](images/architecture_diagram.png) *(Note: Upload your architecture diagram here)*

---

## 📊 Key Findings & Results

Our multi-metric evaluation on the [DeepBacs E. coli Brightfield dataset](https://zenodo.org/records/5550935) reveals a critical **32.3 percentage point gap** between interior region accuracy and actual boundary delineation.

| Metric | Value | Interpretation |
| :--- | :--- | :--- |
| **mAP (0.50:0.95)** | 59.21% | Strong baseline detection performance. |
| **AP50** | 82.56% | Highly successful at localizing ~82% of all cells. |
| **Mean Region IoU** | 0.790 | Excellent interior pixel classification. |
| **Boundary IoU** | **0.467** | Exposes severe boundary contraction at the cell edges. |

### Geometric Shape Preservation
Despite boundary contraction causing a **29.4% underestimation in cell area**, the model successfully preserves scale-independent morphotypes:
* **Eccentricity Error:** Only **0.51%** (Successfully preserves rod-shaped morphology).
* **Solidity Error:** Only **2.88%** (Successfully preserves cell convexity).

![Visual Results](images/phase1_to_phase4_visualization.png) *(Note: Upload your 6-panel visualization here)*

---

## 🚀 Installation & Setup

This pipeline is built for Google Colab or local Linux environments with NVIDIA GPUs.

**1. Clone the repository:**
```bash
git clone [https://github.com/YOUR_USERNAME/EColi-Segmentation-MaskRCNN.git](https://github.com/YOUR_USERNAME/EColi-Segmentation-MaskRCNN.git)
cd EColi-Segmentation-MaskRCNN
