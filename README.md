# Low Default Portfolio Credit Risk Modeling

Probability of Default estimation for Low Default Portfolios using classical Pluto–Tasche, Wilson score intervals and hierarchical modeling.

## Overview

This repository presents my individual implementation and analysis from an academic project conducted in collaboration with **EY Quantitative Advisory Services (QAS)** within Financial Services.

The project focuses on **Probability of Default (PD)** estimation for **Low Default Portfolios (LDPs)**, where the scarcity of observed defaults makes naive empirical estimates unstable and potentially insufficiently conservative.

The objective is to compare several prudent PD estimation approaches and assess how they behave under sparse-default conditions.

## Main methods

The project studies and compares:

- **Empirical Probability of Default**
- **Classical Pluto–Tasche**
- **Wilson score intervals**
- **Hierarchical Pluto–Tasche / Empirical Bayes**
- **Shrinkage across rating segments**

The hierarchical approach is designed to stabilize PD estimates for small or weakly populated segments by sharing information across the portfolio.

## Workflow

The analysis includes:

1. Data cleaning and preprocessing
2. Construction of a lower-risk **semi-LDP**
3. Repeated random sampling to create sparse-default portfolios
4. Classical Pluto–Tasche estimation
5. Wilson score interval estimation
6. Rating-based segmentation
7. Hierarchical PD estimation with shrinkage
8. Comparison of empirical, classical, hierarchical and Wilson estimates

## Key result

In the studied sample:

- the **Wilson upper bound** is the most conservative estimator;
- **classical Pluto–Tasche** is also strongly conservative;
- the **hierarchical Pluto–Tasche approach** provides a smoother and more balanced prudent estimate;
- the **empirical PD** remains the lowest benchmark because it contains no prudential adjustment.

The hierarchical approach is particularly useful for reducing instability in small rating segments while preserving conservative PD estimates.

## Repository structure

```text
low-default-portfolio-credit-risk/
│
├── notebooks/
│   └── credit_risk_ldp.ipynb
│
├── report/
│   └── project_report.pdf
│
├── README.md
├── requirements.txt
└── .gitignore
```

## Technologies

- Python
- pandas
- NumPy
- SciPy
- Matplotlib
- Jupyter Notebook / Google Colab

## Data

The original dataset contains roughly **500,000 loan observations** from 2007 to 2014.

The raw dataset is **not included in this repository**. This avoids committing large source files and keeps the repository focused on the methodology and implementation.

The notebook expects data files to be placed locally in a `data/` directory.

## Reproducibility

Install the required Python packages:

```bash
pip install -r requirements.txt
```

Then open:

```text
notebooks/credit_risk_ldp.ipynb
```

and run the notebook sequentially after placing the required dataset in the local `data/` directory.

## Report

A detailed academic report presenting the mathematical background, methodology, implementation and results is available in:

```text
report/project_report.pdf
```

## Author

**Vincent Haïk Karakoseian**

Financial engineering / quantitative finance project.

## Disclaimer

This repository is provided for academic and portfolio purposes.  
The views, code and analyses presented here are my own and should not be interpreted as official EY research, methodology or advice.
