# Hotel Bar Inventory Forecasting & Par Level Recommendation System

A data-driven inventory forecasting and replenishment recommendation system developed for the **Krystal Ball AI/ML Intern Assignment**. The project forecasts daily alcohol consumption for hotel bars, recommends dynamic inventory **Par Levels**, and evaluates replenishment policies through inventory simulation.

## Business Problem

Hotel bars often face two operational challenges:

- **Stockouts** of high-demand brands, leading to lost sales and poor customer experience.
- **Overstocking** of slow-moving inventory, increasing holding costs and reducing storage efficiency.

This project helps managers make smarter inventory decisions by forecasting demand and recommending dynamic inventory levels.

---

# Project Objectives

- Clean and validate transaction-level inventory data.
- Convert raw transactions into daily Bar-Brand consumption time series.
- Analyze consumption trends and seasonality.
- Compare multiple forecasting models.
- Recommend dynamic inventory **Par Levels**.
- Simulate inventory performance using an Order-Up-To replenishment policy.
- Evaluate stockout risk and inventory efficiency.

---

# Dataset Summary

| Item | Value |
|------|------|
| Records | 6,575 |
| Date Range | 1 Jan 2023 – 1 Jan 2024 |
| Bars | 6 |
| Brands | 16 |
| Alcohol Categories | 5 |

### Key Columns

- Date Time Served
- Bar Name
- Alcohol Type
- Brand Name
- Opening Balance (ml)
- Purchase (ml)
- Consumed (ml)
- Closing Balance (ml)

---

# Project Structure

```text
bar_inventory_project/
├── data/
│   ├── raw/
│   │   └── bar_inventory_data.csv
│   └── processed/
│       ├── daily_bar_consumption.csv
│       ├── abc_brand_classification.csv
│       ├── historical_stockout_analysis.csv
│       ├── dynamic_par_level_recommendations.csv
│       ├── inventory_policy_summary.csv
│       ├── inventory_simulation_daily.csv
│       └── final_inventory_performance_summary.csv
│
├── notebooks/
│   └── inventory_forecasting_solution.ipynb
│
├── report/
│   └── business_report.pdf
│
├── video_script/
│   └── video_walkthrough_outline.md
│
├── requirements.txt
└── README.md
```

---

# Methodology

## 1. Data Preparation

The dataset was validated for:

- Missing values
- Duplicate records
- Negative inventory values
- Invalid dates
- Inventory conservation

Inventory conservation was verified using:

> Closing Balance = Opening Balance + Purchase − Consumed

All records passed the validation within a **10 ml tolerance**.

---

## 2. Time-Series Preparation

- Converted timestamps to daily dates.
- Aggregated consumption by **Date × Bar × Brand**.
- Created a complete daily date grid.
- Filled missing dates with zero consumption.

---

## 3. Exploratory Data Analysis

The notebook includes:

- Consumption by Bar
- Consumption by Brand
- Alcohol category distribution
- Day-of-week patterns
- Weekend vs Weekday demand
- Monthly trends
- ABC inventory classification
- Historical inventory depletion analysis

Historical findings:

- **416** zero-closing-balance records
- **102** records with positive consumption during zero balance
- **26,363.84 ml** associated with inventory depletion events

---

## 4. Forecasting Models

A chronological **80/20 train-test split** was used.

### Models Evaluated

- 7-Day Rolling Mean
- 7-Day Seasonal Naive
- Holt-Winters Exponential Smoothing
- Random Forest Regression

### Model Performance

| Model | MAE (ml) | WAPE (%) |
|------|---------:|---------:|
| 7-Day Rolling Mean | 94.44 | 169.52 |
| 7-Day Seasonal Naive | 97.53 | 175.06 |
| **Holt-Winters** | **91.91** | **164.97** |
| Random Forest | 95.02 | 170.56 |

### Selected Model

**Holt-Winters** was selected because it achieved the lowest MAE and WAPE.

---

## 5. Dynamic Par Level

Inventory recommendations are calculated using:

> Par Level = Forecasted Lead-Time Demand + Safety Stock

Assumptions:

- Lead Time = **2 days**
- Service Level = **95%**
- Z-score = **1.645**

Safety stock is based on forecast variability.

---

## 6. Inventory Simulation

A daily Order-Up-To simulation was performed across the test period.

Simulation Summary:

| Metric | Result |
|------|------:|
| Simulation Period | 74 days |
| Bar-Brand Combinations | 16 |
| Total Observations | 1,184 |
| Simulated Stockout Days | 47 |
| Stockout Rate | 3.97% |
| Simulated Lost Volume | 5,149.46 ml |
| Zero-Stockout Combinations | 7 |
| Combinations Requiring Attention | 9 |

---

# Business Recommendations

- Use dynamic **Bar-Brand-specific Par Levels**.
- Increase monitoring for high-risk combinations.
- Maintain safety stock based on forecast uncertainty.
- Review supplier lead times for repeated stockouts.
- Recalculate Par Levels regularly.
- Monitor inventory turnover alongside stockout events.

---

# Installation

## 1. Clone or Open the Project

Navigate to the project folder:

```bash
cd bar_inventory_project
```

## 2. Create a Virtual Environment

### Windows

```bash
python -m venv .venv
.venv\Scripts\activate
```

## 3. Install Dependencies

```bash
python -m pip install -r requirements.txt
```

---

# Running the Notebook

Start Jupyter:

```bash
jupyter notebook
```

Open:

```text
notebooks/inventory_forecasting_solution.ipynb
```

Run the notebook from top to bottom.

Ensure the raw dataset is available at:

```text
data/raw/bar_inventory_data.csv
```

---

# Video Presentation

The project includes a **3–5 minute walkthrough** explaining:

- Business problem
- Data preparation
- Forecasting approach
- Model comparison
- Dynamic Par Level logic
- Inventory simulation
- Business recommendations

Use the outline available in:

```text
video_script/video_walkthrough_outline.md
```

The recorded OBS presentation serves as the final video deliverable.

---

# Deliverables

- `inventory_forecasting_solution.ipynb`
- `business_report.pdf`
- Video walkthrough (OBS recording)
- `requirements.txt`
- `README.md`

---

# Future Improvements

- Holiday and event-aware forecasting
- Promotional calendar integration
- Supplier lead-time variability modeling
- Automated dashboard for bar managers
- Real-time POS integration
- Data drift monitoring in production

---

# Technologies Used

- Python 3.10+
- Pandas
- NumPy
- Matplotlib
- Seaborn
- Statsmodels
- Scikit-learn
- Jupyter Notebook