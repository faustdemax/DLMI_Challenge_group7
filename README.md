# DLMI Challenge - Group 7  
**Faustine de Maximy & Cyrielle Théobald**

## Introduction

This repository contains the work we conducted for the *Deep Learning for Medical Imaging* course kaggle assignment.  
The goal was to automatically detect cancer metastases in Whole Slide Imaging (WSI) histology slides, while addressing the Out-Of-Distribution (OOD) challenge due to different acquisition centers.

Our final pipeline is based on:
- A pretrained **DINOv2** feature extractor  
- A simple **linear neural network** classifier  
- A strategy involving **data augmentation** and **test-time augmentation (TTA)**  

---

## Repository Structure

### `notebook_preprocessing.ipynb`  
Our baseline pipeline:
- Applies basic augmentations: `ColorJitter`, `RandomHorizontalFlip`, etc.
- Extracts features using **DINOv2**
- Trains a simple linear classifier  
- **Public Kaggle score**: `0.92881`

### `notebook_TTA.ipynb`  
Same pipeline as above, but includes **Test-Time Augmentation** (TTA):
- Improves prediction robustness by averaging predictions over multiple augmented versions
- **Best public Kaggle score**: `0.93573`

### `pretraining-dinov2.ipynb`  
An attempt to finetune **DINOv2** on our dataset (both train, test and val):
- Too computationally expensive (only 1 epoch trained)
- No performance gain observed

### `histogram-matching.ipynb`
Tests **histogram matching** to standardize color distribution across slides:
- Performance decreased, likely due to information loss from color normalization
- **Public Kaggle score**: `0.8737`
  
### `double-input.ipynb` 
 Explores a **double-input strategy** where predictoins are averaged over multiple views or patch inputs:
 - Provded a significant boost over the baseline
 - **Public Kaggle score**: `0.93062`

### `Data_visualisation.ipynb`
Used for exploratory data analysis:
- Visualizes class distribution across training, validation and test sets
- Shows slide distribution across acquisition centers
- Helped guide modeling choices and understand domain shift
  
---

## Methodology

- We used **DINOv2** to extract high-quality features from WSI patches.
- A **linear neural network** is used as the classifier.
- **Data augmentation** (rotation, color jitter, scaling, etc.) is applied during training to improve generalization.
- **Test-Time Augmentation** is applied at inference to stabilize predictions and boost performance.

We also experimented with:
- Histogram matching (resulted in performance drop)
- Double-input strategies for robustness (promising)
- Alternative pretrained models (DenseNet, ResNet) which underperformed compared to DINOv2

---

## Results Summary

| Method                          | Public Kaggle Score |
|----------------------------------|---------------------|
| DINOv2 + NN + TTA               | **0.93573**         |
| Double Input + DINOv2 + NN      | 0.93062             |
| DINOv2 + NN                     | 0.92881             |
| Histogram Matching              | 0.8737              |
| DINOv2 finetuning (1 epoch)     | ~0.5 (failed)       |

---



