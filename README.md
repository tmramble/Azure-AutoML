# Azure-AutoML

# 📈 Bike Rental Demand Prediction with Azure AutoML

## Overview

This project demonstrates an end-to-end machine learning workflow using **Azure Machine Learning Studio**. The goal is to predict hourly **bike rental demand** based on time, weather, and calendar data using **Automated Machine Learning (AutoML)**.

This project includes training, evaluating, and deploying a regression model using Azure services and testing it with real-time data through a deployed REST API.

---

## 🚲 Problem Statement

> How can we predict the number of bikes rented in a given hour based on environmental and temporal features?

Accurate predictions can help businesses optimize bike availability, maintenance, and staffing based on demand forecasts.

---

## 🔧 Technologies Used

- **Azure Machine Learning Studio**
- **Azure Blob Storage**
- **Automated Machine Learning (AutoML)**
- **Azure Container Instances (ACI)**
- **Python (for endpoint consumption)**
- **Regression Algorithms**: LightGBM, RandomForest

---

## 📁 Dataset

- **Source**: [Capital Bikeshare](https://www.capitalbikeshare.com/system-data)
- **Features**:
  - Date/time attributes: season, year, month, hour, weekday, holiday
  - Weather attributes: temperature, humidity, windspeed
  - Target: `rentals` (number of bikes rented)

---

## 🚀 Project Workflow

### 1. Workspace Setup
- Created an Azure ML Workspace to manage compute, data, and models.

### 2. Data Ingestion
- Uploaded and registered the dataset to Azure ML from local files.
- Dataset formatted as a Table (MLTable) for AutoML compatibility.

### 3. Model Training with AutoML
- Selected **Regression** as the task type.
- Used **Normalized Root Mean Squared Error (NRMSE)** as the evaluation metric.
- Allowed only **RandomForest** and **LightGBM** for model selection.
- Limited trials to 3 and enabled early stopping.

### 4. Model Deployment
- Deployed the best-performing model to **Azure Container Instances** as a REST API.
- Created a real-time endpoint named `predict-rentals`.

### 5. Model Testing
- Used the **Test tab** in Azure ML Studio to send a sample JSON payload and receive predictions.

---

## 📦 Sample Input

```json
{
  "input_data": {
    "columns": [
      "season", "year", "month", "hour", "holiday", "weekday", "workingday",
      "weather", "temp", "atemp", "humidity", "windspeed"
    ],
    "index": [0],
    "data": [[3, 1, 1, 0, 0, 6, 0, 1, 9.84, 14.395, 81, 0]]
  }
}

🔁 Sample Output
json
{
  "result": [347.959]
}

The model predicts that approximately 348 bike rentals are expected under the given conditions.

Key Learnings
Hands-on experience with cloud-based ML pipelines

Practical understanding of regression modeling

End-to-end exposure to AutoML training, model selection, and deployment

Introduction to Azure's REST endpoint testing and compute resource management


Next Steps
Improve prediction accuracy by engineering new features (e.g., rush hour indicators)

Build a web or dashboard frontend for non-technical users to interact with the model

Monitor endpoint usage with Application Insights

🧠 Definitions
Regression: A type of ML model that predicts continuous numeric values.

AutoML: Azure service that automates algorithm selection, tuning, and validation.

ACI (Azure Container Instances): Lightweight, serverless containers used for ML model deployment.

NRMSE: Normalized root mean squared error, a common metric to evaluate regression models.

🙋‍♀️ Author
Taylor Ramble
IT Architect Specialist | Cyber Operations Grad Student
📍 Atlanta Public Schools




