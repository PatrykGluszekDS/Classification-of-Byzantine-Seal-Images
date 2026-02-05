# Byzantine Seal Character Classification (TMDRA Project)

Course project for **Technologies for Multimodal Data Representation and Archives (AY 2025/26)**.  
The task is historical OCR as image classification: recognizing cropped Byzantine Greek characters from ancient seal images.

## Project Overview

This project studies character recognition on a difficult cultural-heritage dataset with:
- strong class imbalance,
- heavy visual degradation (erosion, low contrast, noise),
- heterogeneous crop sizes and material variability.

I implemented and compared three model families:
1. **LeNet-300-inspired fully connected baseline**
2. **LeNet-5-inspired CNN** (augmentation + regularization)
3. **ResNet50 transfer learning** (frozen backbone and fine-tuning)

The goal was not only performance, but also a principled, iterative ML workflow.

---

## Dataset

- Source: BHAI Byzantine character crops (24 selected classes)
- Labels are provided in `labels.txt`
- Filename format: `<class_id>__<index>.jpg`
- Typical crop sizes vary widely; all images are resized to **112×112**

> Dataset is included inside `images.zip` file  

---

## Preprocessing Pipeline

- Restructure flat image folders into class-based directories for Keras loaders
- Train/validation split from training data
- Separate held-out test set
- In-model normalization/rescaling
- Data augmentation for CNNs:
  - random rotation
  - random zoom
  - random contrast

---

## Models

### 1) Baseline (LeNet-300 style)
- Flatten + Dense(300) + Dense(100) + Softmax
- Used as a lower bound

### 2) LeNet-5-style CNN (best)
- Conv + AvgPool + BatchNorm blocks
- Dense head + Dropout
- Better spatial feature learning and stronger generalization

### 3) ResNet50 Transfer Learning
- ImageNet-pretrained backbone
- Frozen-backbone training
- Additional fine-tuning of upper layers with lower learning rate

---

## Results (Summary)

- **Best model:** LeNet-5-style CNN
- Clear improvement over dense baseline
- Transfer learning with ResNet50 underperformed due to domain mismatch
- Fine-tuning helped ResNet50 but still did not beat LeNet-5
- Stratified 5-fold cross-validation confirmed stable performance

---

## Repository Structure

```text
├── TMDRA_Project.ipynb
├── TMDRA_project_report.pdf
├── labels.txt
├── data/
│   └── images.zip
└── README.md
```

---

## Acknowledgments

This project was developed for the course **Technologies for Multimodal Data Representation and Archives (TMDRA)** at University of Turin.

Dataset attribution: Byzantine character crops provided for educational use in the TMDRA project context (see course materials and original source references).  

Built with Python, TensorFlow/Keras, scikit-learn, NumPy, and Matplotlib.
