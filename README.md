# Mouse Tracking Deception Detection

A research implementation of mouse tracking as a method for deception detection, including data collection, feature extraction, and neural network classification, with an optimized structure for better development and reproducibility.

## Overview

This repository contains a complete mouse tracking experiment framework reorganized for better modularity and extensibility:
- A Flask-based web application for data collection
- Data preprocessing scripts
- Feature importance analysis
- Neural network training and visualization for deception detection

This is the recommended repository for further development or reproduction of research results from our [playground repository](https://github.com/Bachelor-Thesis-Mobai-2025/mouse_tracking_playground).

## Features

- **Dynamic Layout System**: 5 unique, responsive CSS layouts
- **Mouse Tracking**: Captures detailed mouse movement and interaction data
- **Experimental Design**:
    - Truthful vs. Deceptive response tracking
    - Location-based questioning with randomized question selection
    - Randomized yes/no button placement
    - Comprehensive data logging
- **Data Analysis**:
    - Feature importance extraction
    - Trajectory preprocessing
    - Neural network classification (best model: 68.57% accuracy on test set)

## Project Structure

The repository has been reorganized into a modular structure for better separation of concerns:

```
.
├── Flask/                          # Web application for data collection
│   ├── data/                       # Data storage directory
│   │   ├── deceitful/              # Deceptive response data
│   │   └── truthful/               # Truthful response data
│   ├── static/                     # Static web assets
│   ├── templates/                  # HTML templates
│   └── app.py                      # Flask application entry point
│
├── Torch/                          # Neural network training and evaluation
│   ├── logs/                       # Training logs
│   ├── feature_analysis.py         # Feature importance analysis
│   ├── train.py                    # Model training script (68.57% accuracy)
│   └── truncate.py                 # Trajectory preprocessing
│
├── Images/                         # Visualization outputs
│   ├── Data Collection/            # Data collection interface images
│   ├── Graphs/                     # Data visualization outputs
│   └── Neural Network/             # Model architecture and results
│       ├── Architecture/           # Network architecture diagrams
│       └── Feature Importance/     # Feature importance visualizations
│
├── .gitignore                      # Git ignore file
├── sample.json                     # Sample data format documentation
└── README.md                       # Project documentation
```

## Layouts Showcase

The application randomly selects from 5 unique layouts for each experiment session:

| Layout   | Preview                                           | Description                         |
|----------|---------------------------------------------------|-------------------------------------|
| Layout 1 | ![Layout 1](./Images/Data%20Collection/layout-1.png) | Vertical, clean modern design       |
| Layout 2 | ![Layout 2](./Images/Data%20Collection/layout-2.png) | Horizontal, split-screen approach   |
| Layout 3 | ![Layout 3](./Images/Data%20Collection/layout-3.png) | Innovative corner-positioned layout |
| Layout 4 | ![Layout 4](./Images/Data%20Collection/layout-4.png) | Modern diagonal layout              |
| Layout 5 | ![Layout 5](./Images/Data%20Collection/layout-5.png) | Fully responsive clean layout       |

## Key Components

### Data Collection (`Flask/app.py`)
- Manages experiment flow
- Randomizes question selection for each phase
- Randomizes yes/no button positions
- Logs detailed mouse tracking data
- Supports truthful and deceptive response phases

### Data Preprocessing (`Torch/truncate.py`)
- Trims trajectory data after the last click event and removes initial points where both x and y are 0

### Feature Analysis (`Torch/feature_analysis.py`)
![Feature Importance](./Images/Neural%20Network/Feature%20Importance/top_features_importance.png)
![Feature Importance Matrix](./Images/Neural%20Network/Feature%20Importance/correlation_matrix.png)
- Analyzes feature importance for deception detection
- Most discriminative features include:
    - Acceleration changes
    - Velocity standard deviation
    - Movement smoothness
    - Jerk metrics

## Neural Network
![Neural Network Architecture](./Images/Neural%20Network/Architecture/architecture.svg)

- Best model performance: 68.57% accuracy on test set
- Better network performance noted when using preprocessed data with `truncate.py`
- The model (`train.py`) utilizes StratifiedKFold cross-validation with SMOTE for handling class imbalance

## Model Training Approach

The repository includes a comprehensive training approach in `Torch/train.py`:

1. **Cross-Validation**
    - Implements StratifiedKFold cross-validation (5 folds)
    - Stratification ensures each fold maintains the same class distribution as the original dataset
    - This is critical for imbalanced datasets to prevent training or testing on skewed class distributions
    - Each fold trains on 80% of data and tests on 20% while preserving class ratios
    - The best-performing fold is selected as the final model
    - Provides a more reliable model selection approach compared to a single train/test split

2. **SMOTE-Enhanced Training**
    - Addresses class imbalance using Synthetic Minority Oversampling Technique
    - Creates synthetic samples of the minority class
    - Improves model's ability to detect deceptive movements

The training approach employs early stopping, learning rate scheduling, and focal loss to handle class imbalance.

## Tracked Metrics

The system captures a comprehensive set of mouse movement features:
- Trajectory coordinates (x, y)
- Velocity and acceleration
- Curvature and jerk
- Path efficiency
- Hesitations and pauses
- Directional changes
- Decision timing

## Setup and Installation

1. **Clone the repository**
   ```bash
   git clone <repository-url>
   cd mouse-tracking-optimized
   ```

2. **Create virtual environment**
   ```bash
   python -m venv .venv
   source .venv/bin/activate  # On Windows: .venv\Scripts\activate
   ```

3. **Install dependencies**
   ```bash
   pip install flask pandas numpy matplotlib scikit-learn tqdm seaborn torch imblearn
   ```

4. **Run the web application for data collection**
   ```bash
   cd Flask
   python app.py
   ```

5. **Visualize collected data**
   ```bash
   python graphical_analysis.py
   ```

## Experiment Workflow

1. **Data Collection**
    - Run the Flask application
    - Collect participant responses through the web interface
    - Data is automatically saved to the Flask/data folders

2. **Data Preprocessing**
   ```bash
   cd Torch
   python truncate.py
   ```

3. **Feature Importance Analysis**
   ```bash
   python feature_analysis.py
   ```

4. **Model Training**
   ```bash
   python train.py  # 5-fold CV with SMOTE
   ```

## Research Findings

- Movement dynamics (acceleration change, velocity std) are the most discriminative features
- Path efficiency and movement smoothness show significant differences between truthful and deceptive responses
- The model, at best, has achieved 68.57% accuracy on the test set
- StratifiedKFold cross-validation (5 folds) was used to maintain class distribution across all data splits
- The best-performing fold was selected as the final model
- Visualization outputs stored in the Images directory
- Individual fold performance results available in log directories

## Why Use This Repository?

This repository offers several advantages over the playground version:

1. **Improved Organization**: Clear separation of concerns with a logical directory structure
2. **Better Reproducibility**: Streamlined workflow for reproducing research results
3. **Simplified Extensibility**: Modular architecture makes it easier to extend or modify components

## Technologies Used

- **Backend**: Flask
- **Frontend**: HTML, CSS, JavaScript
- **Data Analysis**:
    - Pandas
    - NumPy
    - Matplotlib
    - Seaborn
- **Machine Learning**:
    - PyTorch (neural network implementation)
    - Scikit-learn (metrics and preprocessing)
    - Imbalanced-learn (SMOTE for class balancing)