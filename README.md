# Scania Predictive Maintenance using Association Rule Mining

[![Python](https://img.shields.io/badge/Python-3.8%2B-blue)](https://www.python.org/)
[![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-orange)](https://jupyter.org/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

> **Data Mining Project **
> Predictive Maintenance for Scania Trucks using PCA and Association Rule Mining

---

## Table of Contents

- [Overview](#overview)
- [Key Features](#key-features)
- [Dataset](#dataset)
- [Methodology](#methodology)
- [Results](#results)
- [Installation](#installation)
- [Usage](#usage)
- [Project Structure](#project-structure)
- [Contributors](#contributors)

---

##  Overview

This project applies **data mining techniques** to predict maintenance needs in **Scania trucks** using real-world multivariate time series data. We transform continuous sensor readings into actionable association rules that help identify failure patterns before they occur.

### **Business Impact**
- Identify high-risk vehicles before failure
- Discover patterns in 23,550 vehicle operational histories
- Generate 2,279 actionable maintenance rules
- Reduce downtime through predictive insights

---

##  Key Features

### **1. Time Series Aggregation**
- Aggregated 1.1M+ operational readouts to vehicle-level features
- Computed statistical metrics: mean, std, min, max, median, last value
- Calculated temporal trends using OLS slope analysis

### **2. Dimensionality Reduction (PCA)**
- Reduced 741 features to 74 principal components
- Retained 95% of variance
- Identified key sensors driving failure patterns

### **3. Association Rule Mining**
- Discovered **49,209 frequent itemsets** using FP-Growth algorithm
- Generated **181,749 association rules**
- Filtered to **2,279 maintenance-predicting rules**
- Achieved confidence levels up to 85% with lift values >2.3×

### **4. Interactive Visualizations**
- Network graphs showing feature-to-failure relationships
- PCA scree plots and cumulative variance charts
- Bar charts ranking top association rules
- Basket heatmaps showing pattern distributions

---

## Dataset

**Source:** Scania Component X Dataset - Industrial Time Series Data

| File | Records | Description |
|------|---------|-------------|
| `train_specifications.csv` | 23,550 | Vehicle specifications (8 categorical features) |
| `train_tte.csv` | 23,550 | Time-to-event labels (repair status, study duration) |
| `train_operational_readouts.csv` | 1,122,452 | Sensor readings (106 features × time steps) |

**Target Variables:**
- `in_study_repair`: Binary indicator (1 = repair needed)
- `length_of_study_time_step`: Duration of monitoring period

---

## Methodology

```
┌─────────────────────────────────────────────────────┐
│ 1. DATA LOADING & AGGREGATION                      │
│    • Load 3 CSV files                               │
│    • Aggregate time series to vehicle level         │
│    • Compute statistics + slopes (741 features)     │
└────────────────────┬────────────────────────────────┘
                     │
┌────────────────────▼────────────────────────────────┐
│ 2. FEATURE ENGINEERING                              │
│    • Drop zero-bin and time features (91 cols)      │
│    • Impute missing values (median/mode)            │
│    • Standardize numerics + one-hot categoricals    │
└────────────────────┬────────────────────────────────┘
                     │
┌────────────────────▼────────────────────────────────┐
│ 3. DIMENSIONALITY REDUCTION (PCA)                   │
│    • 741 features → 74 components (95% variance)    │
│    • Identify important sensors via loadings        │
└────────────────────┬────────────────────────────────┘
                     │
┌────────────────────▼────────────────────────────────┐
│ 4. BASKET TRANSFORMATION                            │
│    • Discretize continuous features (2 quantiles)   │
│    • Create binary transaction matrix               │
│    • Filter by support (≥5%) and correlation        │
│    • Final: 23,550 vehicles × 103 items             │
└────────────────────┬────────────────────────────────┘
                     │
┌────────────────────▼────────────────────────────────┐
│ 5. ASSOCIATION RULE MINING (FP-GROWTH)              │
│    • Min support: 15%                               │
│    • Min confidence: 60%                            │
│    • Max itemset size: 3                            │
│    • Output: 2,279 label-predicting rules           │
└─────────────────────────────────────────────────────┘
```

---

## Results

### **Key Findings**

| Metric | Value | Description |
|--------|-------|-------------|
| **Frequent Itemsets** | 49,209 | Item combinations appearing in ≥15% of vehicles |
| **Association Rules** | 181,749 | Total IF→THEN patterns discovered |
| **Actionable Rules** | 2,279 | Rules predicting maintenance needs |
| **Top Rule Confidence** | 85.2% | Reliability of best predictor |
| **Top Rule Lift** | 2.31× | Strength vs. random chance |

### **Example Top Rule**

```
IF: {397_26_slope_HIGH, Spec_1_Cat1}
THEN: REPAIR_NEEDED

• Support: 22.5% (5,304 vehicles)
• Confidence: 85.2% (highly reliable)
• Lift: 2.31× (strong association)

Interpretation:
When sensor 397_26 shows increasing trend (degradation)
AND vehicle is specification type Cat1
→ Schedule preventive maintenance (85.2% chance of failure)
```

### **Visual Insights**

1. **PCA Analysis:** 74 components capture 95% variance → significant redundancy in original 741 features
2. **Sensor Importance:** Sensors 158, 397, 459 explain most variance
3. **Temporal Trends:** Slope features (degradation over time) are strong predictors
4. **Rule Network:** Clear feature-to-failure pathways visualized

---

## Installation

### **Prerequisites**
- Python 3.8 or higher
- Jupyter Notebook
- 4GB+ RAM recommended

### **Setup**

```bash
# Clone the repository
git clone https://github.com/Sallamhamza/scania-predictive-maintenance-project.git
cd scania-predictive-maintenance

# Create virtual environment (recommended)
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt

# Launch Jupyter Notebook
jupyter notebook notebooks/Scania_Predictive_Maintenance.ipynb
```

---

## Usage

### **Quick Start**

1. **Place your data files** in the `data/` folder:
   - `train_specifications.csv`
   - `train_tte.csv`
   - `train_operational_readouts.csv`

2. **Run the notebook** end-to-end:
   ```bash
   jupyter notebook notebooks/Scania_Predictive_Maintenance.ipynb
   ```

3. **View results** in `outputs/`:
   - `association_rules.csv` - All discovered rules
   - Visualizations (saved plots)

### **Customization**

Modify parameters in the notebook:

```python
# Association Rule Mining Parameters
Q = 2                      # Number of quantile bins (2 = low/high)
MIN_SUPPORT = 0.15         # Minimum frequency (15%)
MIN_CONFIDENCE = 0.60      # Minimum reliability (60%)
TOP_N_BY_LABEL = 100       # Number of features to keep
```

---

## Project Structure

```
scania-predictive-maintenance-project/
│
├── data/                          # Data files (not tracked in git)
│   ├── train_specifications.csv
│   ├── train_tte.csv
│   └── train_operational_readouts.csv
│
├── notebooks/                     # Jupyter notebooks
│   └── Scania_Predictive_Maintenance.ipynb
│
├── outputs/                       # Generated results
│   ├── association_rules.csv
│   └── figures/
│
├── docs/                          # Documentation
│   ├── METHODOLOGY.md
│   └── RESULTS_EXPLAINED.md
│
├── .gitignore                     # Git ignore rules
├── requirements.txt               # Python dependencies
├── README.md                      # This file
└── LICENSE                        # MIT License
```

---

## Documentation

- **[Methodology Guide](docs/METHODOLOGY.md)** - Detailed explanation of techniques
- **[Results Interpretation](docs/RESULTS_EXPLAINED.md)** - How to read the outputs
- **[FAQ](docs/FAQ.md)** - Common questions

---

## Technologies Used

| Technology | Purpose |
|------------|---------|
| **Python 3.8+** | Core programming language |
| **Pandas** | Data manipulation and analysis |
| **NumPy** | Numerical computations |
| **Scikit-learn** | PCA, preprocessing, pipelines |
| **mlxtend** | FP-Growth algorithm, association rules |
| **Matplotlib** | Data visualization |
| **Seaborn** | Statistical plots |
| **NetworkX** | Rule network graphs |

---

## Contributors

** Data Mining Project**

- Member 1 - Data preprocessing & PCA
- Member 2 - Association rule mining
- Member 3 - Visualization & documentation

---

## License

This project is licensed under the MIT License 
---

## Acknowledgments

- **Dataset Source:** Scania Component X Dataset (Industrial Analytics)
- **Course:** Data Mining
- **Institution:** Uppsala University

---

## Contact

For questions or collaborations:
- Email: sallamhamza77@gmail.com
- LinkedIn: https://www.linkedin.com/in/hamza-sallam-b737b7159/

---

## Star this repository if you found it helpful!

