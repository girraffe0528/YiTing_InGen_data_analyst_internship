# Week 2 EDA Memo — Aido Rover Synthetic Telemetry

## Overview

Exploratory data analysis was conducted on the 21-day synthetic telemetry dataset for a fleet of 30 Aido Rovers. The analysis examined sensor distributions, missingness, cross-channel relationships, stationarity, temporal patterns, and spatial behavior. Three findings are particularly relevant to fleet reliability and operational monitoring.

## Finding 1 — GPS Dropout Is Small but Operationally Important

The missingness analysis (Figure 5) shows that missing values are concentrated entirely in the GPS channels. Latitude and longitude each contain approximately **2.0% missing observations**, corresponding to the intentionally injected GPS dropout events. Because the dropout observations were randomly selected without conditioning on rover identity, time, operational mode, or other sensor measurements, the missingness is classified as **Missing Completely At Random (MCAR)**.

This pattern is comparable to temporary telemetry gaps in atmospheric science datasets, where communication or data-logger interruptions can produce missing observations independently of the physical variable being measured.

**InGen operational implication:** Even a 2% GPS dropout rate can temporarily reduce rover localization and route-monitoring reliability. Fleet monitoring systems should flag GPS gaps and use complementary localization or short-term position estimation when GPS telemetry is unavailable.

## Finding 2 — Wheel Torque Channels Show Strong Cross-Sensor Consistency

The cross-channel correlation heatmap (Figure 6) shows that the four wheel torque channels have the strongest positive correlations in the dataset. The highest correlation is between front-left and rear-right wheel torque (**r = 0.867**), while all six wheel-to-wheel correlations range from approximately **0.865 to 0.867**. In comparison, the correlations between LiDAR distance and wheel torque are much weaker, ranging from approximately **0.084 to 0.086**.

The strong wheel-to-wheel relationships are expected because all four wheels respond to the same rover operational mode. A more notable result is that battery SoC and most IMU channels show little linear relationship with wheel torque. In a physical rover, sustained higher torque could contribute to increased energy consumption, so the weak battery-torque relationship highlights a limitation of the synthetic telemetry model.

**InGen operational implication:** Strong agreement among wheel torque channels provides a useful baseline for drivetrain health. Persistent divergence in one wheel relative to the others could indicate a motor, wheel, drivetrain, or sensor problem and could be used as an early maintenance alert.

## Finding 3 — Battery SoC and Wheel Torque Are Statistically Stationary

The time-series analysis for AIDO_001 (Figure 11) shows statistically stationary behavior for both battery SoC and front-left wheel torque. The Augmented Dickey-Fuller test produced an **ADF statistic of -8.7546 with p < 0.001** for battery SoC and an **ADF statistic of -79.6041 with p < 0.001** for front-left wheel torque. In both cases, the null hypothesis of a unit root is rejected at the 5% significance level.

The particularly strong stationarity of wheel torque is consistent with repeated fluctuations around mode-dependent operating levels. Battery SoC is also statistically stationary despite discharge and recharge behavior, partly because operational modes can switch frequently in the synthetic dataset.

**InGen operational implication:** Stable statistical baselines make abnormal behavior easier to detect. Changes in the stationarity or typical operating range of battery and torque telemetry could therefore provide early indicators of battery degradation, mechanical problems, or unusual rover behavior.

## Conclusion

The EDA identifies three useful dimensions of fleet monitoring: GPS data reliability, cross-sensor consistency, and time-series stability. Together, these findings demonstrate how telemetry analytics can support localization reliability, drivetrain health monitoring, and early anomaly detection. The analysis also highlights limitations of synthetic telemetry, particularly the weak physical coupling among some sensor channels, which should be considered when using the dataset for later predictive modeling.
