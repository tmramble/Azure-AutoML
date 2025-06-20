# Azure-AutoML

# 📈 Bike Rental Demand Prediction with Azure AutoML

This project showcases how to build, train, deploy, and test a machine learning regression model using **Azure Machine Learning Studio's Automated ML (AutoML)**. It predicts bike rentals based on environmental and calendar data. I chose this project because it allowed me to train real data with real time endpoints and allowed me a seamless low-code way to incorporate machine learning into my portfolio!

---

## Why This Project?

Bike rental services and city planners need accurate forecasts to allocate bikes efficiently. Regression models help estimate rentals by analyzing patterns in weather, seasons, and time.

---

##  Step 1: Set Up Azure ML Workspace

1. Sign in at [Azure Portal](https://portal.azure.com)
2. Select **+ Create a resource** > Search for **Machine Learning**
3. Use the following settings:
   - **Subscription**: My Azure subscription
   - **Resource group**: New or existing (rg1)
   - **Region**: East US
   - **Workspace name**: Must be unique
  
4. Click **Review + Create** > then **Create**
5. After deployment, click **Launch studio**

![Example](https://github.com/tmramble/Azure-AutoML/blob/main/images/Screenshot%202025-06-20%20073058.png?raw=true)
---

##  Step 2: Upload & Register Dataset

Use historical bike rental data from [Capital Bikeshare](https://aka.ms/bike-rentals).

1. Go to **Automated ML** under *Authoring*
2. Click **New Automated ML Job**
3. Upload dataset:
   - From local files (download from [aka.ms/bike-rentals](https://aka.ms/bike-rentals))
   - Name: `bike-rentals`
   - Format: Table (mltable)
   - Storage: Azure Blob (workspaceblobstore)

 ![Example](https://github.com/tmramble/Azure-AutoML/blob/main/images/Screenshot%202025-06-20%20083323.png)
![Example](https://github.com/tmramble/Azure-AutoML/blob/main/images/Screenshot%202025-06-20%20083127.png)

![Example](https://github.com/tmramble/Azure-AutoML/blob/main/images/Screenshot%202025-06-20%20083358.png)

---
## Validate Data
![Example](https://github.com/tmramble/Azure-AutoML/blob/main/images/Screenshot%202025-06-20%20083455.png)

##  Step 3: Configure AutoML Job

###  Task: Regression

We predict a continuous value (number of rentals), so regression is the appropriate task.

![Example](https://github.com/tmramble/Azure-AutoML/blob/main/images/Screenshot%202025-06-20%20083208.png)

###  Settings:
- **Target column**: `rentals`
The target column (rentals) is an integer that represents a quantity, making regression an appropriate choice over classification or clustering.

- **Primary Metric**: NormalizedRootMeanSquaredError (NRMSE)
Explanation: NRMSE is a scale-independent version of RMSE, making it easier to compare errors across models trained on different datasets or with different target variable scales.

Why it matters: Lower NRMSE indicates better performance. It's used to rank models during automated machine learning (AutoML) runs.

  ![Example](https://github.com/tmramble/Azure-AutoML/blob/main/images/Screenshot%202025-06-20%20083546.png)

  
- **Allowed models**: `RandomForest`, `LightGBM`
  
  RandomForest: A robust, ensemble-based model that handles overfitting well and performs well on structured data.

LightGBM: A gradient boosting framework optimized for performance and speed, especially effective on large datasets with numerical and categorical features.

![Example](https://github.com/tmramble/Azure-AutoML/blob/main/images/Screenshot%202025-06-20%20083738.png)


- **Max trials**: `3`
- Azure ML will try up to 3 different model configurations. This restricts the number of total training attempts, helping conserve compute time and cost.

- **Max concurrent trials**: `3`
- Up to 3 trials can run in parallel, taking advantage of available compute nodes. This maximizes speed and reduces total job duration.

![Example](https://github.com/tmramble/Azure-AutoML/blob/main/images/Screenshot%202025-06-20%20083811.png)

- **Experiment timeout**: `15 mins`
A hard limit for the entire AutoML job. If it runs longer than 15 minutes, all trials are stopped.

- **Iteration timeout**: `15 mins`
- A time limit for each individual model training run (trial). Prevents any one model from consuming excessive time.

Threshold: 0.085 (Normalized Root Mean Squared Error)
The job will terminate early if a model achieves a score equal to or better than 0.085, which represents a low error rate relative to the scale of the data.
**This ensures efficient use of resources — if an excellent model is found early, there's no need to continue running all trials**

![Example](https://github.com/tmramble/Azure-AutoML/blob/main/images/Screenshot%202025-06-20%20083811.png)

- **Early termination**: Enabled
- **Validation**: 10% Train-validation split
- **Compute**: Serverless → `Standard_DS3_v2`, 1 instance

![Example](https://github.com/tmramble/Azure-AutoML/blob/main/images/Screenshot%202025-06-20%20084021.png)

---

##  Step 4: Monitor Results

1. Let training complete
2. View the **Overview** tab → best model summary
   
![Example](https://github.com/tmramble/Azure-AutoML/blob/main/images/Screenshot%202025-06-20%20084111.png)

4. Click the **Algorithm name** for more info
5. Go to **Metrics** tab:
   - Residuals histogram:  The residuals chart shows the residuals (the differences between predicted and actual values) as a histogram
   - Predicted vs True plot: The predicted_true chart compares the predicted values against the true values.
     
![Example](https://github.com/tmramble/Azure-AutoML/blob/main/images/Screenshot%202025-06-20%20084246.png)

---

##  Step 5: Deploy the Model

1. From the best model page, click **Deploy**
2. Settings:
   - **Endpoint type**: Real-time
   - **VM**: Standard_DS3_v2 (or available alternative)
   - **Instance count**: 3
   - **Inferencing**: Disabled
   - **Package model**: Disabled
3. Wait for status to be **Succeeded**

---

## Step 6: Test the Endpoint

1. In **Endpoints**, select your deployed endpoint
2. Go to the **Test** tab
3. Replace the default JSON with:

```json
{
  "input_data": {
    "columns": [
      "day", "mnth", "year", "season", "holiday", "weekday",
      "workingday", "weathersit", "temp", "atemp", "hum", "windspeed"
    ],
    "index": [0],
    "data": [[1,1,2022,2,0,1,1,2,0.3,0.3,0.3,0.3]]
  }
}
```


Click Test, and you’ll receive a prediction like:

**[352.35]**


Step 7: Clean Up Resources

To avoid charges:

Delete the deployed endpoint in ML Studio

Delete the resource group in Azure Portal if done


🧠 Key Takeaways
Used Azure AutoML to automate model selection and tuning

Trained a real regression model on real-world rental data

Deployed to a live, cloud-hosted REST API for testing




