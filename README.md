# Bluebikes Rebalancing Optimization

Demand forecasting, staffing optimization, and cost analysis for the Bluebikes bike-sharing system across Greater Boston.

**Course:** DADS 5250 — Generative AI Hackathon

**Team Members:** Aditi Rao, Satoshi Nakamura, Vinay Manikandan Nagarajan

## Project Overview

Bluebikes is a bike-sharing system operating across Greater Boston. One of its main operational challenges is ensuring that stations have enough bikes available for customers while also maintaining enough open docks for returns.

The goal of this project is to use historical trip data to predict bike rebalancing demand for 2026 and evaluate staffing strategies that balance operational costs with customer demand.

We developed a Python-based application that combines historical demand forecasting, staffing and cost analysis, and event-based demand adjustments using LangChain and the OpenAI API.

## Data Sources

- **Bluebikes Historical Trip Data:** Trip records from 2024–2025 used for forecasting.
- **Bluebikes Station Data:** Station locations, municipalities, and dock capacities.
- **2026 Trip Data:** January–September 2026 records used for backtesting.

Data sources:
- [Bluebikes System Data](https://bluebikes.com/system-data)
- [Historical Trip Data Archive](https://s3.amazonaws.com/hubway-data/index.html)

## Methodology

### 1. Data Preparation and Demand Forecasting

We started by downloading historical trip data and matching bike stations to their municipalities. We then calculated daily station imbalances by comparing the number of bikes picked up and returned.

These daily imbalances were aggregated to estimate monthly rebalancing demand.

To forecast 2026 demand, we used a weighted average of historical demand from the same month in 2024 and 2025, assigning 30% weight to 2024 and 70% to 2025.

### 2. Staffing Optimization and Cost Analysis

Using the demand forecasts, we calculated the number of crews required each month based on the provided operational assumptions.

We compared maintaining existing full-time staffing levels with a revised scenario that reduced staffing in Cambridge and added seasonal staffing in Boston during the summer.

The analysis considered crew capacity, full-time and seasonal costs, and workforce adjustments.

### 3. Event Analysis

We incorporated LangChain, prompt engineering, and structured outputs to evaluate how major local events could affect bike-sharing demand.

The event analyzer extracts information such as event date, location, and expected impact. We tested the workflow using the Boston Marathon and applied an illustrative demand adjustment to evaluate whether staffing requirements would change.

### 4. Backtesting and Sensitivity Analysis

We compared our 2026 predictions against January–September 2026 trip data to evaluate forecast accuracy.

We also tested staffing coverage under three demand scenarios: 20% below observed demand, observed demand, and 20% above observed demand.

## Application Architecture

The application follows this overall workflow:

```text
2024–2025 Bluebikes Trip Data
              |
              v
     Data Cleaning & Station Matching
              |
              v
     2026 Demand Forecasting
              |
              +---------------------------+
              |                           |
              |                  Event Description
              |                           |
              |                           v
              |                  LangChain + OpenAI
              |                           |
              |                           v
              |                  Structured Event Data
              |                           |
              +-------------+-------------+
                            |
                            v
                 Adjusted Demand Forecast
                            |
                            v
                 Staffing & Cost Analysis
                            |
                            v
                   Staffing Recommendations

2026 Actual Trip Data
          |
          v
Backtesting & Sensitivity Analysis
```

The event-analysis component provides an optional adjustment to the demand forecast. Staffing calculations are performed in Python using the project's operational assumptions.

## Key Findings

- Forecasted rebalancing demand across **13 Greater Boston municipalities** for all 12 months of 2026.
- Achieved a weighted absolute percentage error (WAPE) of **13.57%** when comparing forecasts with January–September 2026 demand estimates.
- Identified potential summer staffing shortages in Boston through backtesting.
- Developed a revised four-city staffing scenario with an estimated annual cost of **$834,900**, compared with the $864,000 baseline.
- Identified potential annual savings of **$29,100** while maintaining sufficient estimated capacity for observed January–September demand.
- Found that a 20% increase in observed demand would create 10 understaffed city-months under the revised plan, with approximately 14,375 bike movements exceeding estimated monthly capacity.

## Setup Instructions

1. Download `Bluebikes_Hackathon.ipynb` from this repository.
2. Open the notebook in [Google Colab](https://colab.research.google.com/).
3. Add an OpenAI API key to Colab Secrets under the name `OPENAI_API_KEY` and enable notebook access.
4. Run the notebook cells in order. Required packages are installed in the notebook, and the Bluebikes datasets are downloaded automatically.
5. Review the generated forecasts, staffing comparisons, visualizations, and backtesting results.

The project uses Python libraries including pandas, NumPy, matplotlib, requests, LangChain, Pydantic, and the OpenAI API. Running the GenAI component requires API access and may incur usage costs.

## Project Scope and Limitations

This project uses historical Bluebikes data from 2024–2025 to predict rebalancing demand across 13 municipalities for 2026. While demand was
