# Week 7 Dashboard Memo

## Fleet Analytics Dashboard

The Week 7 Fleet Analytics Dashboard integrates results from the previous analytical phases into a four-panel operational monitoring view. The dashboard was developed in Tableau Public using Aido Rover telemetry and outputs generated from the Week 2–6 analyses.

### 1. Fleet Overview

**Analytical Question:**  
What is the current and overall operational state of the Aido Rover fleet?

**InGen Platform Supported:**  
Aido Rover

**Operator Action:**  
Fleet operators can use this panel to identify the dominant operational modes, check the latest status of all 30 rovers, and identify units with low battery levels that may require operational attention.

**Underlying Analysis:**  
This panel uses the telemetry dataset explored during Week 2 EDA, including operational mode, rover ID, timestamp, and battery State of Charge (SoC). The fleet operated primarily in PATROL mode over the 21-day monitoring period, while the latest telemetry snapshot shows 27 of 30 rovers in PATROL mode.

### 2. Time-Series Monitor

**Analytical Question:**  
How does fleet-average Battery SoC change over time, and how does the forecast compare with observed battery behavior and detected anomalies?

**InGen Platform Supported:**  
Aido Rover

**Operator Action:**  
Operators can monitor battery decline, compare observed Battery SoC with the Holt forecast and its prediction interval, and use hourly anomaly activity to identify periods that may require closer battery monitoring.

**Underlying Analysis:**  
This panel combines the Week 5 time-series analysis with Week 6 anomaly detection. The Holt Exponential Smoothing model provides the final seven-day Battery SoC forecast and 95% prediction interval, while statistical battery anomaly flags are aggregated to the hourly level for comparison with the fleet-level time series.

### 3. Classification Insights

**Analytical Question:**  
How accurately can rover operational modes be classified, and which telemetry features contribute most strongly to the predictions?

**InGen Platform Supported:**  
Aido Rover

**Operator Action:**  
Operators and analysts can identify operational modes that are more difficult to distinguish and prioritize the sensor signals that provide the strongest information for automated fleet-state classification.

**Underlying Analysis:**  
This panel uses the Week 3 Random Forest classification and feature importance analyses. Random Forest achieves F1 scores above 0.86 for all four operational modes. The largest classification error is ALERT predicted as PATROL, while permutation importance identifies wheel imbalance as the strongest predictor, followed by wheel torque features and LiDAR distance.

### 4. Anomaly Alert Summary

**Analytical Question:**  
How effectively are controlled rover faults detected, and where are anomaly alerts concentrated across the fleet?

**InGen Platform Supported:**  
Aido Rover and Sentinel Prime AI

**Operator Action:**  
Fleet operators can compare fault-detection performance, identify rover-hour combinations with higher alert activity, and determine which sensor thresholds generate the largest number of anomaly observations.

**Underlying Analysis:**  
This panel integrates the Week 6 anomaly detection benchmark and statistical threshold analysis. Statistical threshold detection achieves higher mean AUROC than Isolation Forest across all five controlled fault types and reaches an AUROC of 1.000 for GPS disruption, IMU axis failure, and LiDAR saturation. Motor-related thresholds generate the highest number of anomaly observations.

## Operational Value

Together, the four panels connect fleet status, battery forecasting, operational-mode classification, and anomaly detection in a single monitoring interface. The dashboard converts the Week 2–6 analytical results into operational information that can support fleet monitoring, model evaluation, and alert prioritization.