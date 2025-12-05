# California Energy Market Volatility Monitor

**Project Type:** Energy Risk Analysis / ETL Pipeline  
**Author:** Gregory J. Wilson  
**Tech Stack:** Python, Pandas, Matplotlib, CAISO OASIS API

## ⚡ Project Overview
This project is a Proof-of-Concept (PoC) tool designed to monitor **Basis Risk** in the California ISO (CAISO) electricity market. It automates the extraction of pricing data to quantify the "DART Spread"—the price divergence between the Day-Ahead Market (Forecast) and the Real-Time Market (Actual).

For Load Serving Entities (LSEs) and CCAs like **Central Coast Community Energy (3CE)**, monitoring this spread could be used for:
*   **Financial Hedging:** Identifying hours where Real-Time volatility exceeds Day-Ahead commitments.
*   **Operational Compliance:** Verifying market settlement data against grid conditions.


## 🛠️ Technical Approach
The notebook performs a full **ETL (Extract, Transform, Load)** workflow:

1.  **Extract:**
    *   Dynamic construction of API queries using `requests`.
    *   Retrieves Day-Ahead (DAM) and Real-Time (RTM) LMP data for Node `TH_NP15_GEN-APND`.
    *   Handles ZIP file extraction and XML/CSV parsing in memory.
2.  **Transform:**
    *   **Cleaning:** Filters specific price components (Energy vs. Congestion vs. Loss).
    *   **Resampling:** Downsamples high-frequency 5-minute RTM data to Hourly averages to align with DAM schedules.
    *   **Merging:** Joins datasets on UTC timestamps.
3.  **Analyze:**
    *   Calculates the **DART Spread** ($Price_{RealTime} - Price_{DayAhead}$).
    *   Applies a "Buy-Side" risk logic (Positive Spread = Cost Risk).
4.  **Visualize:**
    *   Generates a dual-panel Compliance Dashboard using `matplotlib` and `seaborn`.

## 📊 Sample Visualization
*(Note: Upload the image of your graph to your repo and link it here)*

The tool produces a risk profile where:
*   **Blue/Orange Lines:** Compare Forecast vs. Actuals.
*   **Red Bars:** Indicate hours where the Real-Time price spiked above the forecast (Financial Risk).
*   **Green Bars:** Indicate hours where Real-Time prices dropped below the forecast (Opportunity/Savings).

## 🚀 Future Roadmap
To scale this tool for production Risk Management, the following features are planned:
1.  **Historical VaR:** Loop the extraction process over 12 months to calculate Value-at-Risk (95% Confidence Interval).
2.  **Timezone Standardization:** Implement `pytz` to convert UTC API data to Pacific Prevailing Time (PPT) for trader reporting.
3.  **Automated Alerting:** Script a daily cron job that flags hours where the DART Spread exceeds $50/MWh.

## 📦 Requirements
*   `pandas`
*   `requests`
*   `matplotlib`
*   `seaborn`

## 🤖 Attribution & Tools
*   **Primary Development:** Gregory J. Wilson
*   **AI Assistance:** Google Gemini 3.0 was utilized as a coding assistant for syntax verification, API debugging, and conceptual explanation. This project serves as a demonstration of leveraging AI for rapid upskilling in new domain applications (Hydrology $\to$ Energy).
