
Instruction on running the code and decription of deliveries



1. Prerequisites

Before running the code, ensure you have:

Python 3.8+ installed

A Jupyter Notebook environment (Anaconda / VS Code / Google Colab)

The following libraries installed:

pip install pandas numpy matplotlib seaborn scikit-learn statsmodels faiss-cpu 


2. How to Run the Notebook
Open Jupyter Notebook and navigate to the project folder.

Upload or open the notebook file (e.g., cyclone_analysis.ipynb).

Place your dataset (e.g., data.xlsx) in the same folder as the notebook.

Run all cells sequentially using Run All or Shift + Enter.

Each section (1–6) runs independently and produces visual outputs.



3. Code Sections Overview

 Step    Task                                         Description                                                                                  Output                                           

 1      Data Preparation & EDA           Cleans data, handles missing timestamps, and visualizes 5-min interval behavior.       Summary stats, correlation plots                 
 2      Shutdown Detection              Detects shutdown/idle periods based on temperature threshold.                           Total downtime, number of shutdowns, visual plot 
 3      Machine State Clustering        Groups active data into operational states (Normal, High Load, Degraded).               Cluster assignments, summary stats               
 4      Contextual Anomaly Detection   Finds anomalies within each cluster using Isolation Forest.                              List of anomaly events, visual plots             
 5      Forecasting                    Forecasts inlet gas temperature for next hour using ARIMA and baseline models.           RMSE, MAE, forecast plot                         
 6      Insights & Storytelling          Connects clusters, anomalies, and forecasting for actionable insights.                 CSV summary file, recommendations                


4. Deliverables

       Deliverable                                                     Description                                                                                          
   Jupyter Notebook                       Complete, runnable notebook with data preparation, analysis, clustering, anomalies, and forecasting. 
   Generated Plots                        Visualization of shutdowns, clusters, anomalies, and forecasts.                                      
  insight and storytelling                Summary of 3–5 operational insights and recommendations.                                                                                   




5. Expected Outcomes

A structured, reproducible workflow for analyzing 3-year cyclone data.

Identification of key shutdown patterns and high-risk operating states.

Actionable recommendations for predictive maintenance and monitoring.

