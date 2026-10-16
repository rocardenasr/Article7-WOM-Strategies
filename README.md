# Referrals, Rewards, or Events? Adaptive Word-of-Mouth Marketing Model

[![Python](https://img.shields.io/badge/Python-3.12.14-3776AB?logo=python&logoColor=white)](https://www.python.org/)
[![NumPy](https://img.shields.io/badge/NumPy-2.3.5-013243?logo=numpy&logoColor=white)](https://numpy.org/)
[![pandas](https://img.shields.io/badge/pandas-2.2.3-150458?logo=pandas&logoColor=white)](https://pandas.pydata.org/)
[![Matplotlib](https://img.shields.io/badge/Matplotlib-3.10.8-11557c)](https://matplotlib.org/)

This repository contains the source code, experimental configurations, and data needed to reproduce the computational simulation study described in the article:

> **"Referrals, Rewards, or Events? Resource Constraints and Rule Priority in Adaptive Word-of-Mouth Marketing."**

---

## Overview

This project implements a Python-based **Agent-Based Model (ABM)** to investigate how resource-constrained entrepreneurs should deploy word-of-mouth (WOM) strategies. It compares organic diffusion, fixed policies, a simultaneous mixture, a fixed sequence, and a rule-based adaptive policy within a structured social network of **600 consumer agents** over a **60-month** horizon.

The study addresses constraints of time, budget, and service capacity, specifically analyzing how parameter sensitivities and the ordering of adaptive action rules impact customer acquisition and firm profitability.

---

## Repository Structure

The repository follows the exact organizational structure defined in the project's minimum specification:

```text
Article7-WOM-Strategies/
├── 02_Especificacion_del_modelo/
│   └── Formal model specification, complete ODD protocol, data dictionaries, and decision logs.
├── 03_Codigo/
│   ├── model.py                   # Versioned simulation engine (v0.2.0)
│   ├── rule_orders.py             # Execution scripts for adaptive rule-order permutations (v0.4)
│   └── 02_Pruebas/                # Automated test suite (24 baseline tests, 39 rule-order tests)
├── 04_Diseno_de_experimentos/
│   ├── parameters.csv             # Baseline parameter configurations and seeds
│   ├── agents_initial.csv         # Initial agent attributes for deterministic starting states
│   └── Article_7_Verificacion_v0.2.md  # Verification reports
├── 05_Resultados_brutos/
│   └── Raw output data: pilot runs, main experiments (700 runs), parameter sensitivity (v0.3),
│       rule-order permutations (v0.4), and archives (Article_7_Sensibilidad_datos_v0.3_parte01..08.zip).
├── 06_Datos_procesados_y_analisis/
│   ├── run_summary.csv
│   ├── policy_summary.csv
│   ├── paired_changes_from_baseline.csv
│   ├── paired_order_contrasts.csv
│   └── observed_monthly_series_groups.csv
├── 07_Figuras_y_tablas/
│   └── Editable and publication-ready figures (PNG/SVG), including order_effects and order_actions_and_outcomes.
├── 08_Manuscrito/
│   └── Drafts and submission-ready version of the manuscript.
└── 09_Replicacion_y_suplementos/
    └── Article_7_Colab_v0.2.ipynb # Reproducible execution interface (Jupyter/Colab notebook) & supplements.
Directory Details
02_Especificacion_del_modelo/: Contains the formal model specification, the complete ODD (Overview, Design concepts, Details) protocol, data dictionaries, and decision logs.

03_Codigo/:

Contains the versioned simulation engine (model.py v0.2.0).

Includes the execution scripts for the adaptive rule-order permutations (rule_orders.py v0.4).

02_Pruebas/: Automated testing suite (24 tests for baseline, 39 for rule orders) validating budget constraints, capacity limits, and model consistency.

04_Diseno_de_experimentos/:

parameters.csv and agents_initial.csv containing baseline configurations, random seeds, and initial attributes to reproduce deterministic starting states.

Verification reports (Article_7_Verificacion_v0.2.md, etc.).

05_Resultados_brutos/:

Raw output data from pilot runs, main experiments (700 runs), parameter sensitivity (v0.3), and rule-order permutations (v0.4).

Includes compressed archives such as Article_7_Sensibilidad_datos_v0.3_parte01.zip through parte08.zip.

06_Datos_procesados_y_analisis/:

Derived datasets and analytical results including run_summary.csv, policy_summary.csv, paired_changes_from_baseline.csv, paired_order_contrasts.csv, and observed_monthly_series_groups.csv.

07_Figuras_y_tablas/:

Editable and publication-ready figures in PNG and SVG formats, including order_effects and order_actions_and_outcomes.

08_Manuscrito/:

Drafts and the submission-ready version of the manuscript.

09_Replicacion_y_suplementos/:

Article_7_Colab_v0.2.ipynb: Reproducible execution interface (Jupyter/Google Colab notebook).

Replication instructions and supplementary packages.

Requirements & Environment
The simulation environment was verified using the following exact dependency versions:

Python: 3.12.14

NumPy: 2.3.5

pandas: 2.2.3

Matplotlib: 3.10.8

Installation
To set up the exact environment locally:

Bash
# Clone the repository
git clone [https://github.com/rocardenasr/Article7-WOM-Strategies.git](https://github.com/rocardenasr/Article7-WOM-Strategies.git)
cd Article7-WOM-Strategies

# Install exact dependencies
pip install numpy==2.3.5 pandas==2.2.3 matplotlib==3.10.8
Replication & Usage
Interactive Notebook (Recommended):
Open and run 09_Replicacion_y_suplementos/Article_7_Colab_v0.2.ipynb in Jupyter Lab or Google Colab to execute the pipeline and reproduce the figures and tables.

Command-Line Execution & Testing:
Navigate to 03_Codigo/ to run the simulation scripts (model.py, rule_orders.py) or execute the automated verification suite located in 03_Codigo/02_Pruebas/.
