# Transportation Optimization Analysis Using Linear and Multi-Objective Models

<p align="center">
  <img alt="Python" src="https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white" />
  <img alt="pandas" src="https://img.shields.io/badge/pandas-150458?style=for-the-badge&logo=pandas&logoColor=white" />
  <img alt="NumPy" src="https://img.shields.io/badge/NumPy-013243?style=for-the-badge&logo=numpy&logoColor=white" />
  <img alt="SciPy" src="https://img.shields.io/badge/SciPy-8CAAE6?style=for-the-badge&logo=scipy&logoColor=white" />
  <img alt="PuLP" src="https://img.shields.io/badge/PuLP-00599C?style=for-the-badge&logo=python&logoColor=white" />
  <img alt="Jupyter" src="https://img.shields.io/badge/Jupyter-F37626?style=for-the-badge&logo=jupyter&logoColor=white" />
</p>

<p align="center">
  <b>Freight logistics optimization using linear and multi-objective programming to improve on-time delivery, control cost, and support smarter provider assignment decisions.</b>
</p>

***

## Introduction

This project focuses on transportation and freight optimization using **Python, pandas, NumPy, SciPy, and PuLP** on a real-world logistics dataset. The main goal is to assign trips to transportation providers in a way that improves on-time delivery while also considering cost and vehicle compatibility. [file:7][file:10][file:11]

The repository includes exploratory data analysis, missing value handling, provider performance scoring, trip cost calculation, compatibility mapping, and three optimization models: a single-objective on-time model, a multi-objective cost-performance model, and a multi-objective model with compatibility constraints. [file:7][file:10][file:11]

***

## Project Overview

**Transportation Optimization Analysis Using Linear and Multi-Objective Models** is a freight assignment optimization project built to identify the most reliable and cost-effective providers for trip allocation. The project evaluates historical provider performance and uses linear programming to recommend improved assignments based on operational constraints. [file:7][file:10][file:11]

The workflow includes:

- Exploratory analysis of the transportation dataset.
- Cleaning the `ontime` and `delay` indicators into a binary performance label.
- Computing historical on-time delivery scores for each provider.
- Creating trip-level cost estimates using vehicle type and distance.
- Solving optimization models to maximize reliability and manage cost.
- Introducing compatibility rules between providers and vehicle types. [file:7][file:10][file:11]

***

## Dataset Overview

The dataset contains **5,399 trip records** across **26 transportation providers**. The data shows skewed provider usage, substantial missing values in several columns, and weak correlations among numerical variables, which makes optimization a stronger choice than prediction for this problem. [file:7][file:10][file:11]

Key dataset characteristics include:

- Trip-level shipment and provider records.
- Vehicle types and transportation distance in kilometers.
- On-time and delay indicators.
- Driver, route, and shipment metadata.
- Missingness in provider, driver, and timing fields. [file:7][file:10][file:11]

### Missing Value Handling

The analysis found that the `ontime` and `delay` columns were not clean yes/no labels but instead used values like `G` and missing entries. These were converted into a binary on-time status where `G = 1` and everything else was treated as `0` for optimization purposes. [file:7][file:10][file:11]

***

## Feature Engineering

A provider-level on-time score was created by averaging the binary on-time status for each provider. This score became the main reliability parameter in the optimization models. [file:7][file:10][file:11]

A trip cost was also calculated by mapping each `vehicleType` to a per-kilometer cost and multiplying it by `TRANSPORTATIONDISTANCEINKM`. A default cost was used for vehicle types not explicitly listed in the cost dictionary. [file:10][file:11]

To support the compatibility-based model, vehicle types were grouped into broad categories such as small, medium, and large, and providers were assigned compatibility rules based on those categories. [file:7][file:10][file:11]

### Vehicle Cost Logic

| Component | Description |
|---|---|
| Vehicle cost per km | Assigned using a predefined lookup table based on vehicle type. |
| Default cost | Applied to unlisted vehicle types. |
| Trip cost | Computed as distance multiplied by vehicle cost per km. |
| Compatibility category | Vehicle types were grouped into small, medium, and large classes. |

***

## Modeling Approach

Three optimization models were built using PuLP:

1. **Single-objective on-time model**  
   Maximizes historical on-time performance across all trip assignments.

2. **Multi-objective cost and on-time model**  
   Minimizes a weighted combination of total trip cost and negative on-time score.

3. **Multi-objective model with compatibility**  
   Adds provider-vehicle compatibility constraints to make the solution more realistic. [file:7][file:10][file:11]

The core decision variable in each model is binary, indicating whether a provider is assigned to a trip. Each trip is constrained to exactly one provider. [file:7][file:10][file:11]

### Constraints Used

- Each trip must be assigned to exactly one provider.
- Total cost must remain within the defined budget.
- Providers can only be assigned to compatible vehicle types in the compatibility model. [file:7][file:10][file:11]

***

## Results Summary

The optimization models consistently selected a small group of high-performing providers, especially **WABCOTRANS**, **FORIGOTRANS**, and **BALLY LOGISTICS**. These providers accounted for nearly all optimized assignments in the first two models, showing that historical performance strongly influenced the optimal solution. [file:7][file:10][file:11]

### Single-Objective Model

- Optimal total expected on-time score: **5399.00**
- Assigned trips were concentrated among the highest-performing providers.
- Nearly all assignments changed from the original provider allocation. [file:7][file:10][file:11]

### Multi-Objective Model

- Optimal total expected on-time score: **5399.00**
- Optimal total trip cost: **73,261,161.27**
- Assignments remained heavily concentrated among a few providers.
- The solution balanced cost and reliability under the budget constraint. [file:7][file:10][file:11]

### Multi-Objective with Compatibility

- Optimal total expected on-time score: **5384.00**
- Optimal total trip cost: **73,261,161.27**
- Four providers handled all trips: **WABCOTRANS, FORIGOTRANS, BALLY LOGISTICS, and APACETRANSCO**.
- The slight drop in on-time score came from enforcing realistic compatibility rules. [file:7][file:10][file:11]

***

## Key Findings

- The original provider assignment pattern was not performance-driven.
- Optimization significantly improved expected on-time delivery.
- A small subset of providers consistently outperformed the rest.
- Cost constraints did not prevent strong delivery performance.
- Compatibility rules made the model more practical for real-world use. [file:7][file:10][file:11]

***

## Tech Stack

| Category | Tools |
|---|---|
| Languages | Python |
| Data Handling | pandas, NumPy |
| Optimization | PuLP |
| Statistics | SciPy |
| Visualization | Matplotlib, Seaborn |
| Environment | Jupyter Notebook, Google Colab |
| File Format | Excel, Python script, PDF, PowerPoint |

***

## Repository Structure

```bash
.
├── data/
│   └── Transportation-and-Logistics-Tracking-Dataset.xlsx
├── notebooks/
│   └── optimization_notebook.ipynb
├── scripts/
│   └── optimization_code.py
├── reports/
│   ├── ASDS_6304_Group-1_Final-Project.pdf
│   └── ASDS-6304-Final-Project-3-1.pptx
└── README.md
```

***

## Future Improvements

- Add route-level optimization and multi-stop delivery constraints.
- Introduce provider capacity limits.
- Extend the model with geographic clustering and real-time re-optimization.
- Test alternative objective weights for cost and reliability.
- Incorporate stochastic demand or uncertainty in provider performance. [file:7][file:10][file:11]

***

## Acknowledgments

This project was developed as part of an academic optimization and logistics analysis effort focused on improving freight assignment decisions through data-driven linear programming. [file:7][file:10][file:11]
