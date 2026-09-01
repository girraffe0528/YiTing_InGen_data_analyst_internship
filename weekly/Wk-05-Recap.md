# Week 5 Recap

This week, I applied time-series analysis methods to the Aido Rover battery
State of Charge (SoC) data. The analysis used hourly mean battery SoC across
the 30-unit fleet over a 21-day period and focused on decomposition,
stationarity, lag relationships, and forecasting.

One of the main questions was whether the battery SoC series showed a clearer
periodic structure than the climate time-series patterns I had previously
worked with. I initially expected the battery data to show a noticeable
24-hour cycle because robot operation and charging could potentially follow
daily patterns. However, the results did not show a strong daily periodic
structure. The ACF showed persistent autocorrelation but no clear repeating
peaks at 24-hour intervals, while the STL decomposition showed that the
24-hour seasonal component was relatively small compared with the overall
trend. The most noticeable feature of the battery series was instead its
gradual downward trend over the observation period.

The stationarity analysis also reinforced the importance of testing a
time-series rather than relying only on visual patterns. The raw SoC series
was stationary at the 5% significance level based on the ADF test
(p = 0.0367), while first differencing did not improve the stationarity
evidence. ACF and PACF analysis then identified strong short-term dependence,
particularly at lag 1.

For forecasting, I compared ARIMA(1,0,0) with Holt exponential smoothing
using the first 14 days for training and the final 7 days for testing. Holt
performed slightly better, achieving an MAE of 0.0818 and RMSE of 0.1076.
This suggests that the trend and recent state of the battery series were more
useful for prediction than a strong repeating seasonal cycle.

Overall, this analysis showed that robot energy management can be predictable
even without a clear daily periodic pattern. Short-term persistence and trend
can still provide useful forecasting information. At the same time, the
synthetic dataset and relatively stable final test period mean that these
results should not be generalized directly to real-world robot operations.