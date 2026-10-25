✦ NexusSales AI

Executive Sales Intelligence & Decision Support Platform

Turn sales data into clearer insights, forecasts, and business
decisions.






Sales Analytics · Revenue Forecasting · Executive Insights · Arabic &
English

Overview

NexusSales AI is a Python-based sales intelligence dashboard
designed to help users explore sales performance, understand revenue
patterns, and generate data-driven forecasts through an interactive
executive-style interface.

Users can upload sales files or explore the application with a built-in
synthetic demo dataset, without needing to provide private business
data.

Demo-data note: Sample metrics and forecasts are illustrative
only. They are not real company results.

Key Features

Feature

Description

📂 Data Import

Upload sales data in CSV, XLSX, or XLS format.

🧹 Data Preparation

Clean, standardise, and prepare imported data for analysis.

📊 Executive Dashboard

Review key sales indicators and interactive visualisations.

🔮 Revenue Forecasting

Generate revenue projections using an XGBoost regression model when sufficient historical data is available.

💡 Business Insights

Explore summaries and recommendations to support sales analysis.

🧾 Sales Summary

Review aggregated sales performance through metrics and charts.

🗂️ Data Explorer

Inspect the processed dataset in an interactive table.

💬 Sales Assistant

Ask supported sales questions using the app's built-in rule-based assistant.

🌐 Bilingual Interface

Switch between English and Arabic.

💱 Currency Support

Choose a supported display currency for the dashboard.

🧪 Demo Mode

Explore a synthetic sample dataset without uploading real business data.

Application Sections

Home: Introduction, data upload, and demo-data access.

Dashboard: Sales KPIs and interactive charts.

Forecasts: Revenue projections and evaluation metrics.

Insights: Business-focused observations and recommendations.

Summary: Aggregated sales performance and visualisations.

Data Explorer: A table for inspecting the processed data.

Forecasting Approach

The forecasting workflow aggregates revenue by date and prepares
time-based features, including:

Day of the week, month, and day of the month

Previous-day revenue (lag_1)

Revenue from seven days earlier (lag_7)

Seven-day rolling average

An XGBoost Regressor is used to generate revenue projections. The
app calculates Mean Absolute Error (MAE) and Root Mean Squared
Error (RMSE) for evaluation.

Forecasts are estimates, not guarantees. Their reliability depends on
the amount, consistency, and representativeness of the historical data.

Technology Stack

Technology

Role

Python

Core application logic and data processing

NiceGUI

Interactive web interface

Pandas

Data cleaning, transformation, grouping, and aggregation

NumPy

Numerical operations

Plotly Express & Graph Objects

Interactive charts and visualisations

XGBoost

Revenue forecasting model

scikit-learn

Forecast evaluation metrics

HTML & CSS

Interface structure and styling within the Python application

Run Locally

1. Clone the repository

git clone https://github.com/Shahad58/NexusSales-AI.git
cd NexusSales-AI

2. Create a virtual environment

Windows

python -m venv .venv
.venv\Scripts\activate

macOS / Linux

python3 -m venv .venv
source .venv/bin/activate

3. Install dependencies

Create a requirements.txt file containing the packages used by your
project. For example:

nicegui
pandas
numpy
plotly
scikit-learn
xgboost

Install them with:

pip install -r requirements.txt

4. Start the application

Use the actual name of your Python entry file. If it is named app.py,
run:

python app.py

Then open the local address shown in the terminal.

Important: A README describes the project but does not run or host
it. To let reviewers launch the app themselves, the repository must
also contain the required Python source files and dependency list.

Screenshots

Add screenshots of your actual application to a folder named
screenshots, then use the following format to display them here:

![NexusSales AI Dashboard](screenshots/dashboard.png)

Recommended screenshots:

Home page

Executive dashboard

Revenue forecasting page

Insights or data explorer page

Project Goals

Make sales data easier to explore and interpret.

Present key metrics and trends through interactive visualisations.

Demonstrate a practical machine-learning forecasting workflow.

Bring data preparation, analysis, and reporting into one interface.

Support both Arabic and English users.

Limitations & Responsible Use

Demo data is synthetic and does not represent a real company.

The sales assistant responds to a limited set of supported question
types; it is not a general-purpose generative AI chatbot.

Forecasting results should be reviewed alongside business context and
data quality.

Do not upload confidential customer or company information to a public
demo.

About This Project

NexusSales AI is a portfolio project demonstrating the integration of
Python-based data processing, interactive dashboards, visual analytics,
and machine-learning-based revenue forecasting in one application.

Built with Python · Designed for clearer sales decisions
