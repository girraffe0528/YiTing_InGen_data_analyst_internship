# Week 8 Retrospective

## Atmospheric Science Method Transfer

The analytical method that transferred most naturally from atmospheric science to Physical AI telemetry was time-series analysis. In atmospheric science, observations are collected repeatedly over time, and understanding temporal structure is important for distinguishing long-term trends, recurring patterns, and short-term variability. The same analytical thinking was useful when examining Aido Rover battery State of Charge (SoC).

In the capstone analysis, fleet-average battery SoC was evaluated using STL decomposition, stationarity testing, ACF and PACF analysis, and forecasting. These methods helped identify the underlying trend and temporal dependence in the telemetry before comparing forecasting approaches. Although the physical meaning of robot battery data is very different from atmospheric measurements, the analytical workflow transferred well because both involve repeated observations whose interpretation depends on time.

## Most Surprising Classification Finding

The most surprising classification result was that class weighting did not improve detection of the minority FAULT class. Because FAULT represented only 4.94% of the observations, I initially expected a class-weighted Random Forest to improve FAULT recall.

However, the baseline Random Forest achieved a FAULT recall of 0.7858, while the class-weighted model achieved a lower recall of 0.7680. Macro-F1 also decreased slightly from 0.9298 to 0.9277. This result showed me that techniques designed to address class imbalance should not automatically be assumed to improve minority-class performance. Their effectiveness depends on the structure of the data and should be evaluated empirically using appropriate class-level metrics.

## Where the Environmental Analogy Breaks Down

One important lesson from the internship was that similarities between environmental data and Physical AI telemetry are methodological rather than literal. For example, battery SoC can be analyzed as a time series in the same way that an environmental variable can be analyzed over time, but battery SoC is not equivalent to an atmospheric measurement. Its behavior is influenced by robot operation, charging logic, mission activity, and system design.

The same limitation applies to anomaly detection. An unusual environmental observation and a robot sensor fault may both appear as deviations from normal patterns, but they have different causes and operational consequences. Treating them as directly equivalent could lead to incorrect interpretations.

The most useful transfer from Environmental Science to Physical AI is therefore the analytical process: validating sensor data, exploring multivariate relationships, examining temporal behavior, identifying abnormal observations, and communicating results through visualization. The domain interpretation must still be based on the operational characteristics of the Physical AI system.