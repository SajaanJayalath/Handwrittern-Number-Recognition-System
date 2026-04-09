# Handwritten Character Recognition System

A Python application for recognizing handwritten **digits**, **letters**, and **arithmetic expressions**. Includes a GUI for drawing/uploading images and separate utilities for math expression solving.

**Course**: COS30018 Intelligent Systems 
---

## Quick Start

### Install Dependencies
```bash
pip install -r requirements.txt
```

### Run GUI
```bash
# Main recognition app (digits, letters, arithmetic)
python src/main.py --gui

# Arithmetic solver (Maths folder)
python Maths/main.py --gui
```

### CLI Options
```bash
python src/main.py --check-deps          # Check dependencies
python src/main.py --test-models         # Test available models
python src/main.py --test-image <path> --model cnn|svm|rf  # Test on single image
```

---

## Features

### Main App (`src/`)
- **3 Recognition Modes**: Digits, Letters, Arithmetic expressions
- **Multiple Models**: CNN, SVM, Random Forest, k-NN
- **Segmentation Methods**: Contours, Connected Components, Projection, Watershed
- **Interactive GUI**: Draw or upload images, visualize results with confidence scores

### Arithmetic Solver (`Maths/`)
- **Separate GUI** for math expression recognition and solving
- Identifies handwritten arithmetic operations
- Solves expressions and displays results
- Includes trained models and evaluation tools

---

## Project Structure

```
src/                              # Main recognition system
├── main.py                       # CLI/GUI entry point
├── gui.py                        # Tkinter interface
├── models.py                     # CNN, SVM, Random Forest
├── image_preprocessing.py        # Image normalization
├── image_segmentation.py         # Character detection
├── data_loader.py                # Dataset loading
├── arithmetic/                   # Arithmetic pipeline
│   ├── pipeline.py
│   ├── models.py
│   └── segmentation.py
├── letters_new/                  # Letters pipeline
│   ├── pipeline.py
│   ├── model_knn.py
│   └── segment.py
└── integrations/
    └── mer_adapter.py            # External MER bridge

Maths/                            # Arithmetic operations & solving
├── main.py                       # GUI launcher
├── gui.py                        # Math expression GUI
├── model.py                      # ML models for math
├── predict.py                    # Prediction module
├── preprocess.py                 # Image preprocessing
├── Data/                         # Training/test data
└── models/                       # Pre-trained models

models/                           # Pre-trained weights
├── cnn_model.h5
├── svm_model.pkl
├── rf_model.pkl
└── label_mapping*.json

data/                             # Datasets
├── by_class/                     # NIST SD19 (letters)
├── emnist/
└── svhn/
```

---

## Requirements

**Python 3.10 or 3.11**

### Dependencies
```bash
numpy
pandas
opencv-python
pillow
scikit-learn
matplotlib
tensorflow           # For CNN features
torch, torchvision   # Optional: for dataset loaders
```

Install all: `pip install -r requirements.txt`

---

## Usage Guide

### Main App Modes

#### Digits Recognition
- **Segmentation**: Auto / Contours / Connected Components / Projection
- **Models**: CNN, SVM, Random Forest
- **Best for**: Digits 0-9

#### Letters Recognition  
- **Segmentation**: Watershed-based
- **Models**: k-NN or CNN
- **Best for**: Alphanumeric characters

#### Arithmetic Expressions
- **Segmentation**: Best / Hybrid / Connected Components / Projection
- **Models**: CNN, Ensemble, or external MER
- **Best for**: Math expressions (e.g., "2+3", "5x7")

### Maths Folder
Run the separate arithmetic GUI:
```bash
python Maths/main.py --gui
```
- Specialized for handwritten math recognition
- Identifies operations and solves expressions
- Alternative to the arithmetic mode in main app

---

## Models & Datasets

### Available Models
| Mode | Model File | Label Mapping |
|------|---|---|
| Digits | `cnn_model.h5` | `label_mapping.json` |
| Letters | `cnn_model_nist_by_class.h5` | `label_mapping_nist_by_class.json` |
| Arithmetic | `cnn_model_arithmetic.h5` | `label_mapping_arithmetic.json` |

### NIST Dataset (Letters)
If you have NIST SD19 by_class data:
```bash
python -c "
import sys
sys.path.append('src')
from letters_new.model_knn import train_knn_byclass, save_model
m, mapping = train_knn_byclass(max_per_class=400, n_neighbors=3)
save_model(m, mapping)
print('Model saved to models_new/letters_byclass_knn.pkl')
"
```

---

## Troubleshooting

| Issue | Solution |
|-------|----------|
| **TensorFlow not found** | `pip install tensorflow==2.15` |
| **No model available** | Ensure model `.h5` or `.pkl` files are in `models/` directory |
| **Low confidence results** | Try different segmentation method or ensure good image quality |
| **Letters mode shows only k-NN** | Add `cnn_model_nist_by_class.h5` to `models/` directory |
| **Arithmetic mode missing** | Add `cnn_model_arithmetic.h5` and label mapping to `models/` |

---

## Optional: External MER Integration

To use external Math-Expression-Recognizer-and-Solver:

1. Clone repository into: `external/Math-Expression-Recognizer-and-Solver` or `src/external/`
2. Ensure `math-recog-model.h5` is present
3. GUI will show "MER (External)" option in Arithmetic mode

---

## Author

**Sajaan Jayalath**

## License

MIT
# 🎯 Handwritten Character Recognition System (HNRS)

> A comprehensive Python application for recognizing handwritten digits, letters, and arithmetic expressions using multiple ML/DL backends.

[![Python 3.10+](https://img.shields.io/badge/Python-3.10%2B-blue)](https://www.python.org/)
[![TensorFlow](https://img.shields.io/badge/TensorFlow-2.x-orange)](https://www.tensorflow.org/)
[![License MIT](https://img.shields.io/badge/License-MIT-green)]()

## 📋 Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Quick Start](#quick-start)
- [Repository Structure](#repository-structure)
- [Requirements](#requirements)
- [Running the App](#running-the-app)
- [Models and Weights](#models-and-weights)
- [Datasets](#datasets)
- [Troubleshooting](#troubleshooting)

---

## 🎨 Overview

HNRS is a Python application for recognizing handwritten digits, letters, and arithmetic expressions. It provides an interactive **Tkinter GUI** to draw or upload images, plus a **CLI** for testing. Under the hood, it combines robust preprocessing, multiple segmentation strategies, and several classifier backends **(CNN, SVM, Random Forest, k-NN)**. 

Developed for **COS30018 Intelligent Systems** at Swinburne University.

## ✨ Features

- **Multi-mode recognition**: Digits, Letters, or Arithmetic expressions
- **Advanced segmentation**: Contours, Connected Components, Projection, Watershed (letters)
- **Multiple models**: 
  - 🧠 CNN (TensorFlow/Keras)
  - 📊 SVM (scikit-learn)
  - 🌳 Random Forest (scikit-learn)
  - 📍 k-NN (letters pipeline)
  - 🔗 Ensemble (arithmetic)
- **Interactive GUI**: 
  - ✏️ Draw/erase directly on canvas
  - 📁 Upload images from file
  - 🔍 Visualize preprocessing & segmentation
  - 📈 Per-character confidence scores
- **External integration**: Optional MER (Math Expression Recognizer) for enhanced arithmetic recognition

## 🚀 Quick Start

### Installation

```bash
# Clone/download the project
cd "path/to/Handwritten Number Recognition System/Source Code"

# Install dependencies
pip install -r requirements.txt
```

### Launch GUI (Default)

```bash
python src/main.py --gui
```

### CLI Commands

```bash
# Check dependency status
python src/main.py --check-deps

# Test available models
python src/main.py --test-models

# Recognize a single image
python src/main.py --test-image path/to/image.png --model cnn|svm|rf|ensemble
```

---

## 📁 Repository Structure

```
├── src/                              # Main source code
│   ├── main.py                       # CLI/GUI entry point
│   ├── gui.py                        # Tkinter interface
│   ├── models.py                     # CNN, SVM, Random Forest models
│   ├── image_preprocessing.py        # Preprocessing utilities
│   ├── image_segmentation.py         # Segmentation & multi-digit processing
│   ├── data_loader.py                # Dataset loaders (MNIST, EMNIST, SVHN)
│   ├── arithmetic/                   # Arithmetic expression pipeline
│   │   ├── pipeline.py
│   │   ├── models.py
│   │   ├── segmentation.py
│   │   └── preprocess.py
│   ├── letters_new/                  # Letters recognition pipeline
│   │   ├── pipeline.py
│   │   ├── model_knn.py
│   │   ├── segment.py
│   │   └── preprocess.py
│   └── integrations/
│       └── mer_adapter.py            # External MER integration
│
├── models/                           # Pre-trained weights & label maps
│   ├── cnn_model.h5
│   ├── svm_model.pkl
│   ├── rf_model.pkl
│   ├── cnn_model_arithmetic.h5
│   └── label_mapping*.json
│
├── models_new/                       # Additional models
│   └── letters_byclass_knn.pkl
│
├── data/                             # Dataset storage
│   ├── by_class/                     # NIST SD19 dataset
│   ├── emnist/
│   └── svhn/
│
├── Maths/                            # Legacy arithmetic sandbox
│
├── requirements.txt                  # Python dependencies
└── README.md                         # This file
```

### Key Modules

| Module | Purpose |
|--------|---------|
| `src/models.py` | CNN, SVM, Random Forest implementations |
| `src/image_preprocessing.py` | Normalization, resizing, enhancement |
| `src/image_segmentation.py` | Contour detection, digit/character splitting |
| `src/arithmetic/` | Expression segmentation, recognition, solving |
| `src/letters_new/` | Watershed segmentation, k-NN + CNN models |
| `src/integrations/mer_adapter.py` | Bridge to external MER-and-Solver |

---

## 📦 Requirements

**Python 3.10 or 3.11** recommended


## ▶️ Running the App

### GUI Mode (Default)

```bash
python src/main.py --gui
```

**GUI Features:**
- ✏️ **Draw Mode**: Draw/erase digits or letters on canvas
- 📁 **Upload Mode**: Load images from file
- 🔄 **Mode Selector**: Switch between Digits, Letters, Arithmetic
- 🎛️ **Segmentation Options**: Choose best method for your input
- 🤖 **Model Selector**: Pick CNN, SVM, Random Forest, or k-NN
- 📊 **Results Panel**: View predictions with confidence scores
- 🖼️ **Visualization**: See preprocessing and segmentation steps

### CLI Mode

```bash
# Check dependency status and install missing packages
python src/main.py --check-deps

# Load models and run smoke test
python src/main.py --test-models

# Recognize a single image
python src/main.py --test-image path/to/image.png --model cnn|svm|rf|ensemble
```

### GUI Tips & Best Practices

#### Digits Mode
- **Segmentation**: Auto, contour-based, connected components, or projection
- **Models**: CNN, SVM, Random Forest
- **Tip**: Use darker, well-separated digits for best results

#### Letters Mode
- **Segmentation**: Watershed-based
- **Models**: k-NN or CNN (if available)
- **Tip**: Similar letterforms (O/0) may need manual verification

#### Arithmetic Mode
- **Segmentation**: Best/Hybrid/Connected Components/Projection
- **Models**: CNN, Ensemble, or external MER
- **Tip**: Ensure clear spacing between characters in expressions

---

## 🧠 Models and Weights

### Model Location

Models and label mappings should be placed in `models/` or `src/models/`. The app searches both directories.

### Available Models

| Mode | Primary Model | Fallback | Label Mapping |
|------|---|---|---|
| **Digits** | `cnn_model.h5` | SVM, Random Forest | `label_mapping.json` |
| **Letters** | `cnn_model_nist_by_class.h5` | k-NN | `label_mapping_nist_by_class.json` |
| **Arithmetic** | `cnn_model_arithmetic.h5` | Ensemble | `label_mapping_arithmetic.json` |

### Pre-trained Model Files

- **CNN Models**:
  - `cnn_model.h5` - CNN model for digits (MNIST)
  - `cnn_model_mnist_csv.h5` - Alternative CNN (MNIST CSV variant)
  - `cnn_model_nist_by_class.h5` - CNN for letters (NIST)
  - `cnn_model_arithmetic.h5` - CNN for arithmetic expressions

- **Classical ML Models**:
  - `svm_model.pkl` - SVM classifier
  - `rf_model.pkl` - Random Forest classifier
  - `models_new/letters_byclass_knn.pkl` - k-NN for letters

- **Metadata**:
  - `label_mapping*.json` - Character-to-index mappings
  - `arithmetic_confusion_matrix.npy` - Evaluation metrics

---

## 📚 Datasets

### NIST SD19 (Letters)

The k-NN letters pipeline expects NIST SD19 `by_class` images organized by class:

```
data/by_class/by_class/
├── 0000/  (digit 0)
├── 0001/  (digit 1)
├── ...
├── 0041/  (letter A)
├── ...
└── 36f/   (letter z)
```

### Train a quick k-NN letters model

If you have NIST by_class data and want a fast letters baseline for the GUI:

```bash
python - <<"PY"
import sys, os
sys.path.append('src')
from letters_new.model_knn import train_knn_byclass, save_model
m, mapping = train_knn_byclass(max_per_class=400, max_total=20000, n_neighbors=3)
save_model(m, mapping)
print('Saved models_new/letters_byclass_knn.pkl')
PY
```

### Other Supported Datasets

- **MNIST CSV**: Used by training scripts in `src/data_loader.py`
- **EMNIST/SVHN**: Available via torchvision (optional, for experimentation)


**Checklist**:
- [ ] TensorFlow is installed: `python -c "import tensorflow; print(tensorflow.__version__)"`
- [ ] Model file exists and is readable
- [ ] Label mapping JSON is in the same directory as model
- [ ] File permissions are correct
- [ ] Model weights are compatible with current TensorFlow version



**Last Updated**: 2026 | **Project**: COS30018 Intelligent Systems