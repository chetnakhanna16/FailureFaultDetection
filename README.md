# FailureFaultDetection
Failure and fault point detection for machines using time series

## Problem Statement:
To develop an automated method to pinpoint the times of fault and failure in machines whose signal data is known

## Exploratory Data Analysis:
The graph depicting all the signals of a machine for 3000 observations can not provide much information. So, I plotted 300 observations in one graph to analyze it better. All graphs can be seen in the jupyter notebook.
<B>Observations:</B>
1. The data has a lot of noise due to communication errors.
2. The signals for the machine during the fault phase are very different from the normal phase. The signals in the fault phase have high frequency than the signals in the normal
phase.
3. The signals for the machine during the failure phase are around zero.

## Pre-Processing:
Steps followed with reasons:
1. The data type of the date column was ‘object’ so it is changed to ‘datetime’.
2. DateTime column has been set as the index for the time series analysis.
3. Data has been verified to be stationary using the ‘Augmented Dickey-Fuller Test’ as it is a simple and strong statistical test to know about stationarity.
4. Outliers have been replaced with the rolling mean value. The reason for choosing ‘rolling mean’ is that it smoothes out short-term fluctuations in time series data and highlights
the long-term trends. A rolling window of 60 observations (meaning around 20 days) has been taken to replace the outliers in each region. Any value that does not lie in +/- two
standard deviations of the mean has been replaced so that I still have around 95% of the actual data and 5% of the replaced data.

## Algorithm and Automation:
Steps followed with reasons:
1. Our problem has data from four different signals for each machine. Since the problem is multivariate time series analysis/forecasting, so I am using the statistical model, Vector Auto Regression (VAR). VAR captures the relationship between multiple quantities as
they change over time.
2. The model has been designed such that it follows the trend it saw when the machine was working in the normal mode and make future predictions. Both the signals (actual
and predicted) are then analyzed.
3. The peaks for the actual signals and the predicted signals have been calculated using ‘argrelextrema’. The reason for choosing the idea of peaks is based on the initial data
exploration that frequency of signals increases when the machine starts moving from the normal phase to the fault phase.
4. Peaks for both actual and predicted signals are monitored for a range of 25 consecutive observations. The point where the difference between the peaks is more than 1.5 times
the predicted peak is taken as the start point of fault for the signal by deeply analyzing the graphs.
5. The mean value of all these signals is the fault start day for the machine. The date corresponding to that day number is the fault start date. In order to inform the concerned
department about the possibility of upcoming failure, I am considering a fault window of two days.
6. For the failure point, the mean of the absolute values of each row is calculated. The first point where the mean is around 0 is considered as the failure point. The date
corresponding to that day number is the failure start date.

## Limitations/Next Steps:
1. There is a scope of hyperparameter tuning for this approach which can possibly improve the results. For example, rolling window and minimum periods for rolling mean, order for
argrelextrema, maximum lags for the VAR model etc.
2. The outliers have been removed by taking the assumption that the signals follow a normal distribution. However, the same can vary from machine to machine, so this may
require some monitoring/tweaking before generalization.
3. A lot of other graphs like autocorrelation, particle autocorrelation etc can be plotted to understand the data better and use a better model to predict the fault and failure
behavior.
4. There is a scope to try the LSTM model to detect the fault behavior of the machine as it works well in case of sequence prediction problems by nicely fitting on the data and
provides good predictions by learning the context provided by the signals.
5. I found a few research papers about change-point detection. I think there is a scope to understand those methods and try them out.
6. I tried to run the code for all the files and the code is failing at a few places. As per my knowledge, this is due to failures in finding the peaks as argrelextrema doesn't consider
repeated values to detect peaks. So, there is definitely an area of improvement in the code I wrote to find the peaks.
