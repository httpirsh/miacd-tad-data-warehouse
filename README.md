# Tourism Supply and Demand Analysis in Portugal

![Python](https://img.shields.io/badge/Python-Data%20Processing-blue)
![Data Warehouse](https://img.shields.io/badge/Data%20Warehouse-Star%20Schema-green)
![Status](https://img.shields.io/badge/Status-Academic%20Project-lightgrey)

## Project Overview

This project analyzes the relationship between **tourism demand** and **accommodation supply** in Portugal, focusing on **Local Accommodation (Alojamento Local)** and **tourism overnight stays**.

The objective is to build a **Data Analytics solution** that combines a **Data Warehouse (DW)** and **Data Mining (DM)** techniques to analyze tourism patterns and support decision-making related to tourism planning and accommodation growth.

The analysis focuses on the period **2011–2024**, integrating multiple public datasets to explore tourism dynamics across Portuguese regions.

---

# Motivation

Tourism is one of the most important sectors of the Portuguese economy. However, the rapid growth of **Local Accommodation (AL)** can create pressure on cities and local populations.

This project aims to answer questions such as:

- How has tourism demand evolved over time?
- Which regions have the highest accommodation capacity?
- Are there areas experiencing excessive tourism pressure?
- Where should accommodation supply be encouraged or regulated?

By combining multiple datasets and analytical methods, this project provides **insights for tourism planning and decision support**.

---

# Data Sources

The project integrates three main datasets.

## 1. Local Accommodation Dataset (RNAL)

Dataset containing information about registered **Local Accommodation establishments in Portugal**, representing the **tourism supply**.

Main variables:

- Registration date  
- Opening date  
- Accommodation type (apartment, house, etc.)  
- Maximum capacity (number of guests)  
- Municipality (Concelho)  
- District  
- NUTS II region  
- NUTS III region  

---

## 2. Tourism Overnight Stays (INE)

Dataset with the number of **overnight stays in tourism accommodation establishments**, representing **tourism demand**.

Main variables:

- Year  
- Region (NUTS / municipality)  
- Number of overnight stays  
- Guest origin (Portugal / Foreign)

---

## 3. Resident Population (INE)

Dataset containing **annual estimates of resident population by municipality**, used to contextualize tourism pressure relative to the local population.

Main variables:

- Year  
- Municipality / Region  
- Resident population

---

# Project Architecture

The project follows a typical **data analytics pipeline**:

```
Data Sources
     │
     ▼
ETL Process
(Extract, Transform, Load)
     │
     ▼
Data Warehouse
(Star Schema)
     │
     ▼
OLAP Analysis
     │
     ▼
Data Mining & Insights
```

---

# Data Warehouse (DW)

The Data Warehouse is designed using a **Star Schema** to support analytical queries.

## Fact Tables

- **Fact_Accommodation**
  - Number of accommodation registrations
  - Total accommodation capacity

- **Fact_Tourism**
  - Number of overnight stays

- **Fact_Population**
  - Resident population

## Dimension Tables

- **Dim_Time**
- **Dim_Region**
- **Dim_Accommodation_Type**

This structure enables efficient analysis across **time, location, and accommodation characteristics**.

---

# Data Analytics Requirements

## Descriptive Analytics

Examples of descriptive questions:

- How has the number of **Local Accommodation registrations** evolved by municipality?
- Which municipalities have the **highest accommodation capacity**?
- How are **tourism overnight stays distributed across NUTS II regions**?
- Which municipalities have the **highest accommodation capacity per resident**?

---

## Diagnostic Analytics

Examples of diagnostic questions:

- Is there a relationship between **growth in Local Accommodation and tourism demand**?
- Which municipalities show **higher tourism pressure relative to population**?
- Are there regions with **high tourism demand but limited accommodation capacity**?

---

# Data Mining (DM)

## Predictive Analytics

Predictive models will be used to estimate:

- Future **growth of tourism overnight stays by region**
- Probability of **new Local Accommodation registrations** in municipalities

Possible techniques include:

- Regression models  
- Time series forecasting  
- Machine learning models  

---

## Prescriptive Analytics

Based on the results, the project aims to suggest:

- Regions where **accommodation supply should increase**
- Municipalities where **tourism growth may require regulation or planning measures**

---

# Expected Outcomes

The project aims to produce:

- A structured **Data Warehouse for tourism analytics**
- Analytical queries and visualizations
- **Predictive insights** about tourism demand
- **Decision support recommendations** for tourism planning

---

# Technologies

Possible technologies used in the project:

- Python
- Pandas / NumPy
- SQL
- PostgreSQL or SQLite
- ETL scripts
- OLAP queries
- Data visualization tools

---

# Project Deliverables

The project will be developed in three stages:

1. **Theme Proposal**
2. **Poster and Presentation**
3. **Final Report and Project Discussion**

The final deliverable includes:

- Data Warehouse implementation
- ETL pipeline
- Analytical queries
- Data mining models
- Final report and presentation materials

---

# Repository Structure

```
project/
│
├── data/                # Raw datasets
├── etl/                 # ETL scripts
├── dw/                  # Data warehouse schema
├── analysis/            # Analytical queries and notebooks
├── models/              # Data mining models
├── visualizations/      # Charts and dashboards
└── README.md
```

---

# Acknowledgements

This project was developed for the **Data Analytics Technologies (TAD)** course at the **University of Coimbra – Department of Informatics Engineering**.

AI tools may have been used to assist with writing, documentation, and coding, in accordance with the course guidelines.