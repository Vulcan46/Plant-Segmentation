# Phenocyte Segmentation Project (Arabidopsis)

This repository contains the codebase for segmenting *Arabidopsis thaliana* saplings into five distinct classes: Background, Leaf, Stem, Seed, and Root. The project evolved from a standard U-Net baseline (V1) to a highly optimized weighted-loss model (V4) capable of segmenting thin, difficult structures like roots and stems.

## Dataset Privacy Notice

**IMPORTANT:** The dataset used for training and testing this model is **not included** in this repository. Due to privacy constraints and data usage policies placed by **Iowa State University (ISU)**, the raw image data cannot be made publicly available.

## File Descriptions

### Core Training Notebooks

* **`PlantSeg.ipynb` (V1 - Baseline):** The initial implementation using a standard U-Net with ResNet34 encoder. This version used standard loss functions and struggled significantly with class imbalance, failing to detect small classes like Stem and Seed (Dice ~0.17).

* **`PlantSeg_v3.ipynb` (V3 - Hybrid Loss Experiment):** An experimental version that attempted to use a Hybrid Loss (Dice + Hausdorff). This version proved unstable due to "spiky" gradients from the Hausdorff component, leading to convergence issues.

* **`PlantSeg_v4.ipynb` (V4 - Weighted Dice - **Best Model**):** The most successful iteration. It implements a **Weighted Dice Loss** with aggressive class weighting `[1, 7, 5, 5, 7]` to penalize the model for ignoring small classes. This achieved excellent results, raising Stem Dice from 0.17 to 0.64 and drastically reducing boundary errors.

* **`PlantSeg_Splitleaf_v5.ipynb` (V5 - Split Leaf):** An experimental branch designed to handle cases where leaves are split into separate instances (Left/Right) to better model overlaps.

* **`Plantseg_v6_distweights.ipynb` (V6 - Distance Weights):** An experimental version incorporating a distance-map-based loss function (pixel-wise weighting based on distance to the nearest boundary) to sharpen predictions without the instability of Hausdorff loss.

### Utilities & Analysis

* **`PlantSeg_DataAnalysis.ipynb`:** A dedicated notebook for analyzing the dataset statistics. It generates boxplots of pixel distributions per class and identifies "bad" masks (missing classes) to filter the test set.
