# UI Sketch Element Classification

A computer vision project for classifying hand drawn user interface elements using convolutional neural networks and transfer learning.

The project compares a custom CNN trained from scratch with EfficientNetB0 transfer learning and selective fine tuning to understand how pretrained visual features perform on UI sketches.

## Overview

UI design often starts with rough sketches and low fidelity wireframes. These sketches are useful for communicating ideas quickly, but they are difficult for software systems to interpret automatically.

This project explores whether deep learning can recognize individual UI components such as buttons, text fields, cards, sliders, checkboxes and menus from hand drawn images.

The workflow covers data preparation, model training, transfer learning, fine tuning and model evaluation.

## Dataset

The project uses the UISketch dataset.

| Property         |     Value |
| ---------------- | --------: |
| Images           |    19,898 |
| Classes          |        21 |
| Input size       | 224 × 224 |
| Training split   |       70% |
| Validation split |       15% |
| Test split       |       15% |

The dataset uses a stratified 70/15/15 split so that class proportions remain approximately consistent across training, validation and test sets.

The original dataset is not included in this repository.

## Approach

The project uses the same data split, image resolution and evaluation process for all models to support a fair comparison.

The workflow includes:

1. Dataset extraction and image validation
2. Class and image path mapping
3. Stratified training, validation and test splitting
4. TensorFlow input pipeline creation
5. Image augmentation
6. Custom CNN baseline training
7. EfficientNetB0 transfer learning
8. Selective EfficientNetB0 fine tuning
9. Model evaluation and error analysis

## Models

### Custom CNN

A lightweight CNN was trained from scratch to establish a baseline.

The architecture contains three convolutional stages followed by global average pooling, dropout and a 21 class softmax output layer.

### EfficientNetB0 with Frozen Backbone

EfficientNetB0 pretrained on ImageNet was used as a feature extractor.

The convolutional backbone remained frozen while a new classification head was trained for the 21 UI classes.

### Fine Tuned EfficientNetB0

The final model selectively fine tunes the last 30 layers of EfficientNetB0 while keeping earlier layers frozen.

This allows deeper visual features to adapt to UI sketches without retraining the entire network.

## Results

All models were evaluated on the same fixed test set.

| Model                     |   Accuracy |   Macro F1 |
| ------------------------- | ---------: | ---------: |
| Custom CNN                |     49.41% |     48.25% |
| EfficientNetB0 Frozen     |     70.12% |     69.81% |
| EfficientNetB0 Fine Tuned | **74.27%** | **74.23%** |

The fine tuned EfficientNetB0 model achieved the strongest overall performance.

Compared with the custom CNN, accuracy improved by 24.86 percentage points and macro F1 improved by 25.98 percentage points.

## Evaluation

The models were evaluated using accuracy, macro precision, macro recall, macro F1 score, confusion matrices, classification reports and misclassified examples.

Macro F1 was particularly useful because every class contributes equally to the final score.

## Key Findings

Transfer learning produced a clear improvement over training a CNN from scratch.

The frozen EfficientNetB0 model already performed substantially better than the custom CNN, showing that pretrained visual features can transfer effectively to hand drawn sketches.

Selective fine tuning improved performance further by adapting deeper EfficientNet features to sketch specific patterns.

Most remaining errors occurred between visually similar UI elements such as text fields and text areas, buttons and chips, dropdown menus and menus, and enabled and disabled switches.

These results suggest that additional layout or contextual information may help distinguish classes with similar visual structures.

## Technologies

Python
TensorFlow
Keras
EfficientNetB0
NumPy
Pandas
scikit learn
Matplotlib
Seaborn
Pillow
Jupyter Notebook

## Project Structure

```text
UI_Sketch_Element_Classification/
│
├── README.md
├── requirements.txt
├── .gitignore
├── ui_sketch_element_classification.ipynb
│
└── docs/
    ├── ui_sketch_element_classification_presentation.pdf
    └── ui_sketch_element_classification_paper.pdf
```

## Running the Project

Clone the repository:

```bash
git clone https://github.com/aX200800/UI-Sketch-Element-Classification.git
cd UI-Sketch-Element-Classification
```

Install the required packages:

```bash
pip install -r requirements.txt
```

Place the UISketch dataset archive in the project directory as `archive.zip`, or extract the dataset into the expected local dataset folder.

Start Jupyter Notebook:

```bash
jupyter notebook ui_sketch_element_classification.ipynb
```

## Limitations

The current system classifies isolated UI elements rather than complete interface layouts.

Evaluation uses a held out portion of the same dataset distribution, so performance on completely different sketch datasets has not been measured.

Some UI categories remain difficult to distinguish because they share similar visual structures.

## Future Work

Future improvements could include Grad CAM explainability, stronger sketch specific augmentation, comparison with MobileNet, ResNet and Vision Transformer architectures, and object detection for complete UI wireframes.

## Authors

**Anjali Barvaliya**
**Vishal Chitroda**

Hochschule Furtwangen University
Image Processing and Computer Vision
SoSe 2026
