# Week 5 Time-Series Memo

## Decomposition

Time-series decomposition separates an observed series into trend, seasonal,
and residual components so that different sources of temporal variation can
be examined independently. In atmospheric science, this approach helps
separate longer-term climate variability, recurring seasonal patterns, and
short-term noise. The same methodology was applied to the hourly
fleet-average Aido Rover battery SoC using STL decomposition. The battery
series showed a clear overall downward trend across the 21-day period, while
the 24-hour seasonal component was relatively weak. This suggests that the
longer-term change in fleet-average SoC was more prominent than a repeating
daily charging cycle.

## Stationarity

Stationarity testing determines whether the statistical behavior of a
time-series is sufficiently stable for modeling or whether transformations
such as differencing are required. This is an important step when analyzing
atmospheric time-series and was applied to the battery SoC series using the
Augmented Dickey-Fuller (ADF) test. The raw battery series produced an ADF
statistic of -2.9809 and a p-value of 0.0367, indicating stationarity at the
5% significance level. First differencing did not improve the stationarity
evidence (p = 0.6085), so the raw series was retained for forecasting. In an
operational setting, establishing this temporal behavior helps determine how
battery observations should be prepared before building forecasting models.

## ACF and PACF

Autocorrelation analysis is used to identify how strongly current observations
are related to earlier observations. In climate analysis, lag relationships
can reveal delayed temporal connections between environmental conditions and
later responses. For the Aido Rover battery series, the ACF showed strong
temporal persistence, while the PACF showed a dominant relationship at lag 1.
This indicates that the current fleet-average battery SoC is strongly related
to its recent state. The observed lag structure supported testing an
AR(1)-type model, represented by ARIMA(1,0,0), as an initial forecasting
approach.

## Forecasting

Forecasting extends the temporal patterns identified in historical data to
estimate future conditions while accounting for uncertainty. In atmospheric
science, this principle is used to anticipate future climate states or related
environmental outcomes. For the Aido Rover analysis, the first 14 days were
used for training and the final 7 days for out-of-sample evaluation.
ARIMA(1,0,0) achieved an MAE of 0.0966 and RMSE of 0.1231, while Holt
exponential smoothing achieved a lower MAE of 0.0818 and RMSE of 0.1076.
Holt was therefore selected as the final forecasting model, and prediction
intervals were used to represent forecast uncertainty. The same time-series
framework can therefore support fleet-level energy monitoring by using past
battery behavior to estimate future SoC and inform recharge planning.
