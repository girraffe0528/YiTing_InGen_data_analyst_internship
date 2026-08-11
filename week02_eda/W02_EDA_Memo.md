# Week 2 EDA Memo

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

## Finding 3 — Statistical Stationarity Does Not Necessarily Indicate Healthy Operation

The time-series analysis for AIDO_003 (Figure 11) shows different operational behavior between battery SoC and front-left wheel torque. Battery SoC declines from approximately 80% to near 0% by around July 6 and remains at a very low level afterward, while wheel torque continues to fluctuate around its normal operating range with occasional large spikes. Despite this visual pattern, the Augmented Dickey-Fuller test produced an **ADF statistic of -8.7546 with p < 0.001** for battery SoC and **-79.6041 with p < 0.001** for front-left wheel torque. The unit-root null hypothesis is therefore rejected for both series.

This result demonstrates that statistical stationarity should not be interpreted as operational stability. In particular, the battery series exposes a limitation of the synthetic data generation process: charging is assigned independently of battery SoC rather than being triggered by low battery levels. Because discharge modes occur more frequently than charging, many observations remain near the lower battery boundary for much of the simulation.

**InGen operational implication:** Statistical tests should be combined with visual and operational checks when monitoring rover health. A stationary telemetry signal can still represent undesirable behavior, such as a battery remaining near 0%. Battery thresholds and charging-state logic should therefore be monitored alongside statistical anomaly indicators.

## Conclusion

The EDA identifies three useful dimensions of fleet monitoring: GPS data reliability, cross-sensor consistency, and time-series stability. Together, these findings demonstrate how telemetry analytics can support localization reliability, drivetrain health monitoring, and early anomaly detection. The analysis also highlights limitations of synthetic telemetry, particularly the weak physical coupling among some sensor channels, which should be considered when using the dataset for later predictive modeling.