# Azure-AutoML

# 📈 Bike Rental Demand Prediction with Azure AutoML

This project showcases how to build, train, deploy, and test a machine learning regression model using **Azure Machine Learning Studio's Automated ML (AutoML)**. It predicts bike rentals based on environmental and calendar data — perfect for your portfolio or LinkedIn.

---

## 🔧 Why This Project?

Bike rental services and city planners need accurate forecasts to allocate bikes efficiently. Regression models help estimate rentals by analyzing patterns in weather, seasons, and time.

---

## ✅ Step 1: Set Up Azure ML Workspace

1. Sign in at [Azure Portal](https://portal.azure.com)
2. Select **+ Create a resource** > Search for **Machine Learning**
3. Use the following settings:
   - **Subscription**: Your Azure subscription
   - **Resource group**: New or existing
   - **Region**: East US
   - **Workspace name**: Must be unique
   - **Other fields**: Leave defaults (Azure will create supporting services)
4. Click **Review + Create** > then **Create**
5. After deployment, click **Launch studio**

![Example]([https://github.com/tmramble/Azure-AutoML/blob/main/Screenshot%202025-06-20%20073026.png](https://github.com/tmramble/Azure-AutoML/blob/main/images/Screenshot%202025-06-20%20073058.png?raw=true))

---

## ✅ Step 2: Upload & Register Dataset

Use historical bike rental data from [Capital Bikeshare](https://aka.ms/bike-rentals).

1. Go to **Automated ML** under *Authoring*
2. Click **New Automated ML Job**
3. Upload dataset:
   - From local files (download from [aka.ms/bike-rentals](https://aka.ms/bike-rentals))
   - Name: `bike-rentals`
   - Format: Table (mltable)
   - Storage: Azure Blob (workspaceblobstore)

{SCREENSHOT}

---

## ✅ Step 3: Configure AutoML Job

### 🎯 Task: Regression

We predict a continuous value (number of rentals), so regression is the appropriate task.

### 🔍 Settings:
- **Target column**: `rentals`
- **Metric**: `Normalized Root Mean Squared Error (NRMSE)`
- **Allowed models**: `RandomForest`, `LightGBM`
- **Max trials**: `3`
- **Max concurrent trials**: `3`
- **Experiment timeout**: `15 mins`
- **Iteration timeout**: `15 mins`
- **Early termination**: Enabled
- **Validation**: 10% Train-validation split
- **Compute**: Serverless → `Standard_DS3_v2`, 1 instance

{SCREENSHOT}

---

## ✅ Step 4: Monitor Results

1. Let training complete
2. View the **Overview** tab → best model summary
3. Click the **Algorithm name** for more info
4. Go to **Metrics** tab:
   - Residuals histogram
   - Predicted vs True plot

{SCREENSHOT}

---

## ✅ Step 5: Deploy the Model

1. From the best model page, click **Deploy**
2. Settings:
   - **Endpoint type**: Real-time
   - **VM**: Standard_DS3_v2 (or available alternative)
   - **Instance count**: 3
   - **Inferencing**: Disabled
   - **Package model**: Disabled
3. Wait for status to be **Succeeded**

{SCREENSHOT}

---

## ✅ Step 6: Test the Endpoint

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

Click Test, and you’ll receive a prediction like:

[352.35]

📸 Screenshot suggestion: Test tab showing input + prediction

Step 7: Clean Up Resources
To avoid charges:

Delete the deployed endpoint in ML Studio

Delete the resource group in Azure Portal if done

📸 Screenshot suggestion: Endpoint delete confirmation

🧠 Key Takeaways
Used Azure AutoML to automate model selection and tuning

Trained a real regression model on real-world rental data

Deployed to a live, cloud-hosted REST API for testing

🙋‍♀️ About the Author
Taylor Ramble
IT Architect Specialist | Cyber Operations Grad Student
📍 Atlanta Public Schools
🔗 LinkedIn • 🌐 Portfolio


