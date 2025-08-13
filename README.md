# 📦 Time Series Forecasting & Supply Optimization

## 📌 Project Overview
This project integrates **time series demand forecasting** with **supply chain optimization** to improve operational efficiency.  
We simulate a multi-tier distribution network and use forecasting techniques to predict product demand at store level, followed by an optimization step to determine **cost-effective transportation routes**.

## 🏗 Supply Chain Setup
- **Plants:** 3 manufacturing plants  
- **Distribution Centres (DCs):** 5  
- **Stores:** 20 retail locations  
- **Products:** 30 SKUs  
- **Time Frame:** 30 weeks of demand data  

Transportation costs are defined for:
1. **Plant → Distribution Centre**  
2. **Distribution Centre → Store**

## 📊 Forecasting Models Used
1. **Exponential Smoothing (ES)** – Captures level & trend  
2. **ARIMA** – Autoregressive Integrated Moving Average  
3. **SARIMA** – Seasonal ARIMA for seasonal demand patterns  

**Model Evaluation Metric:**  
Mean Absolute Error (**MAE**)  
\[
\text{MAE} = \frac{1}{n} \sum_{i=1}^{n} |y_i - \hat{y}_i|
\]

## 🧠 Weighted Ensemble Model (Linear Regression)
Instead of manually assigning weights or using inverse-error weighting, this project uses a **Linear Regression model** to learn the optimal weights for combining predictions from ARIMA, SARIMA, and ES.

Let:
\[
\hat{y}^{ARIMA}_t, \hat{y}^{SARIMA}_t, \hat{y}^{ES}_t
\]
be the forecasts for time $t$ from each model.

The Linear Regression equation is:
\[
y_t = \beta_0 + \beta_1 \hat{y}^{ARIMA}_t + \beta_2 \hat{y}^{SARIMA}_t + \beta_3 \hat{y}^{ES}_t + \epsilon_t
\]

The final ensemble forecast is:
\[
\hat{y}^{Ensemble}_t = \beta_0 + \beta_1 \hat{y}^{ARIMA}_t + \beta_2 \hat{y}^{SARIMA}_t + \beta_3 \hat{y}^{ES}_t
\]

Here:
- $\beta_1, \beta_2, \beta_3$ = learned weights for each forecasting model  
- $\beta_0$ = intercept term  
- $\epsilon_t$ = error term

**Improvement Percentage:**
\[
\text{Improvement}(\%) = \frac{\text{MAE}_{\text{best}} - \text{MAE}_{\text{ensemble}}}{\text{MAE}_{\text{best}}} \times 100
\]

## 🔄 Forecasting Approach
- **Single-step sliding window forecasting**
- Uses the **previous 4 weeks** of demand data to predict the next week's demand  
- Repeats the process iteratively until the final week

Example:
- Weeks 1–4 → Predict Week 5  
- Weeks 2–5 → Predict Week 6  
- ... continue until Week 30

## 🚚 Optimization Model
Objective:
\[
\min \sum_{i,j,k} x_{ijk} \cdot c^{P \to DC}_{ijk} + \sum_{j,s,k} y_{jsk} \cdot c^{DC \to S}_{jsk}
\]

**Constraints:**
- Demand satisfaction at each store
- Flow conservation at each DC
- Non-negativity of decision variables

## 📂 Project Structure
