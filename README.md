# Comparative Explainable Deep Learning for Microplastic Detection and Localisation

This repository contains the source code and supporting artefacts for an MSc Applied Artificial Intelligence research project at Sheffield Hallam University.

**Project title:** Comparative Explainable Deep Learning for Microplastic Detection and Localisation  
**Student:** Yogeshwar Rao Busi  
**Student ID:** 35048180  
**Supervisor:** Jacyana Nunes  
**Course:** MSc Applied Artificial Intelligence  

---

## 1. Project Overview

This project investigates whether deep learning object-detection models can detect and localise microplastic particles in secondary environmental sample images. The project uses an existing public Kaggle dataset containing microscopy-style images and CSV bounding-box annotations.

The aim is not to classify microplastic morphology such as fibre, film, fragment or pellet. The dataset contains a single class, **Microplastic**, so the project is framed as a **single-class object detection and localisation task**.

The final deliverable is a reproducible Kaggle/PyTorch notebook that performs:

- Dataset loading and annotation validation
- Exploratory data analysis
- Image and bounding-box preprocessing
- Training of three object-detection models
- Quantitative model evaluation
- Threshold sensitivity analysis
- Prediction visualisation
- Occlusion sensitivity explainability
- Export of figures, metrics and model evidence

---

## 2. Research Question

> How effectively can comparative deep learning object-detection models detect and localise microplastic particles in secondary environmental sample images, and how can visual explainability support interpretation of model predictions?

---

## 3. Dataset

**Dataset:** Microplastic Dataset for Computer Vision  
**Source:** Kaggle  
**Dataset link:** https://www.kaggle.com/datasets/imtkaggleteam/microplastic-dataset-for-computer-vision  
**Data type:** Secondary image dataset with CSV bounding-box annotations  
**Class label:** Microplastic  

### Dataset Summary

| Item | Value |
|---|---:|
| Total unique images | 781 |
| Total bounding-box annotations | 7,126 |
| Training images | 577 |
| Validation images | 204 |
| Training annotations | 5,425 |
| Validation annotations | 1,701 |
| Number of classes | 1 |
| Human participants | 0 |

The dataset does not provide a separate independent test split, so all model results are reported as **validation performance**, not final unseen test performance.

---

## 4. Ethics and Data Governance

This project follows a secondary-data-only research design.

- No human participants were recruited.
- No interviews, surveys or observations were conducted.
- No personal data were collected or processed.
- No human tissue, bodily fluids or sensitive participant data were used.
- The approved ethics route was UREC1.
- Dataset source and licence evidence should be retained with the submission package.

The project should not be described as involving user testing, expert feedback or participant evaluation unless additional ethics approval is obtained.

---

## 5. Models Implemented

Three object-detection models were implemented and compared using the same dataset and evaluation pipeline.

| Model | Type | Purpose |
|---|---|---|
| Faster R-CNN ResNet50-FPN | Two-stage detector | Strong localisation and recall baseline |
| RetinaNet ResNet50-FPN | One-stage detector | Precision-recall balance using focal loss |
| SSD/SSDLite MobileNetV3 | Lightweight detector | Lower-resource comparison model |

This comparison was selected to evaluate different detector families: a region-proposal model, a dense one-stage model and a lightweight detection model.

---

## 6. Evaluation Metrics

The project uses object-detection metrics rather than simple classification accuracy.

| Metric | Purpose |
|---|---|
| Precision | Measures how many predicted boxes are correct |
| Recall | Measures how many actual microplastic particles are detected |
| F1-score | Balances precision and recall |
| Matched IoU | Measures overlap between predicted and ground-truth boxes |
| Count MAE | Measures average object-count error |
| Threshold sensitivity | Shows how model performance changes with confidence threshold |
| Visual prediction checks | Compares predicted boxes against ground-truth boxes |
| Occlusion sensitivity | Shows image regions that influence detection confidence |

Accuracy and ROC-AUC are not the main metrics because this is not a simple image-classification task. It is an object-detection task where localisation quality, missed particles and false detections matter.

---

## 7. Main Results

### Final Validation Performance at Default Confidence Threshold 0.30

| Model | Precision | Recall | F1-score | Count MAE |
|---|---:|---:|---:|---:|
| Faster R-CNN ResNet50-FPN | 0.6022 | 0.7654 | 0.6741 | 3.1618 |
| RetinaNet ResNet50-FPN | 0.8247 | 0.5973 | 0.6928 | 2.6324 |
| SSD/SSDLite MobileNetV3 | 0.7385 | 0.1129 | 0.1958 | 7.2500 |

### Key Findings

- **RetinaNet** achieved the best default-threshold F1-score and the lowest object-count error.
- **Faster R-CNN** achieved higher recall and became the strongest model after threshold tuning.
- **SSD/SSDLite** was lightweight but missed many microplastic particles, making it less suitable without further tuning.
- Threshold selection had a clear effect on model ranking and practical interpretation.
- Occlusion sensitivity provided a visual explanation of regions influencing model confidence.

---

## 8. Repository Structure

A suggested repository structure is shown below. File names may be adjusted to match the final uploaded files.

```text
comparative-explainable-microplastic-detection/
│
├── README.md
├── notebooks/
│   └── comparative_explainable_microplastic_detection.ipynb
│
├── outputs/
│   ├── figures/
│   ├── metrics/
│   ├── predictions/
│   └── xai/
│
├── reports/
│   ├── supporting_report.pdf
│   └── presentation.pdf
│
└── requirements.txt
```

Recommended submitted artefacts:

- Executed Kaggle notebook
- Output ZIP file
- Exported figures
- Metric CSV files
- Supporting report
- Presentation slides
- Ethics evidence
- Publication procedure form

---

## 9. How to Run the Notebook in Kaggle

1. Open Kaggle Notebooks.
2. Add the dataset:
   `imtkaggleteam/microplastic-dataset-for-computer-vision`
3. Enable GPU acceleration.
4. Upload or open the project notebook.
5. Run the notebook from the first cell to the last cell.
6. Check the exported outputs in `/kaggle/working/`.

The notebook is designed to create an output package containing figures, metrics and model evidence.

---

## 10. How to Run Locally

Local execution is possible, but Kaggle is recommended because the dataset path and GPU environment are easier to manage there.

### Basic setup

```bash
git clone https://github.com/Yogeshwar1205/comparative-explainable-microplastic-detection.git
cd comparative-explainable-microplastic-detection
pip install -r requirements.txt
```

### Main Python packages

```text
python
numpy
pandas
matplotlib
pillow
opencv-python
torch
torchvision
scikit-learn
```

If running locally, download the dataset from Kaggle and update the dataset path in the notebook configuration section.

---

## 11. Explainability Method

The project uses **occlusion sensitivity** as a model-agnostic explainability method. This method masks parts of an image and observes how much the detector confidence changes.

High-sensitivity regions indicate areas that had stronger influence on the model prediction. The heatmap should be treated as an interpretability aid, not scientific confirmation that a region is definitely microplastic.

---

## 12. Limitations

The main limitations are:

- The dataset contains only one class: Microplastic.
- No independent test split is available.
- Validation results may not generalise to all real-world sample conditions.
- Some microplastic particles are small, faint or crowded, making recall difficult.
- Occlusion sensitivity supports interpretation but does not replace expert validation.
- No human or expert feedback was collected because the project remained within UREC1 scope.

---

## 13. Future Work

Possible future improvements include:

- Using a larger and more diverse dataset
- Adding an independent test split
- Testing YOLO-family detectors
- Using segmentation masks if available
- Adding morphology labels such as fibre, film, fragment or pellet
- Improving detector-specific explainability
- Adding model calibration analysis
- Developing a Streamlit or cloud demo after further validation
- Conducting expert validation only after appropriate ethics approval

---

## 14. Academic Integrity and AI Use

Generative AI support was used only in an assistive capacity for planning, wording clarity and explanation support. The project results, metrics and figures are based on the executed Kaggle notebook and documented outputs.

The student remains responsible for:

- Dataset selection
- Ethical compliance
- Notebook execution
- Result checking
- Interpretation
- Final academic submission

---

## 15. Citation

If referencing this project, cite it as:

```text
Busi, Y. R. (2026). Comparative Explainable Deep Learning for Microplastic Detection and Localisation. MSc Applied Artificial Intelligence Research Project, Sheffield Hallam University.
```

Dataset citation:

```text
Momeni, M. (n.d.). Microplastic Dataset for Computer Vision [Data set]. Kaggle. https://www.kaggle.com/datasets/imtkaggleteam/microplastic-dataset-for-computer-vision
```

---

## 16. Contact

**Student:** Yogeshwar Rao Busi  
**Student ID:** 35048180  
**Programme:** MSc Applied Artificial Intelligence  
**Institution:** Sheffield Hallam University
