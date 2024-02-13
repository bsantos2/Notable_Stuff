---
attachments: [Clipboard_2024-01-18-20-53-01.png, Clipboard_2024-01-18-20-58-35.png, Clipboard_2024-01-18-21-01-26.png, Clipboard_2024-01-18-21-03-33.png, Clipboard_2024-01-18-21-16-19.png, Clipboard_2024-01-18-21-18-16.png, Clipboard_2024-01-18-21-19-03.png, Clipboard_2024-01-18-21-21-56.png, Clipboard_2024-01-18-21-28-00.png, Clipboard_2024-01-18-21-30-57.png, Clipboard_2024-01-18-21-32-25.png, Clipboard_2024-01-18-21-34-43.png, Clipboard_2024-01-18-21-43-36.png, Clipboard_2024-01-18-21-45-47.png, Clipboard_2024-01-18-21-48-47.png, Clipboard_2024-01-18-21-54-02.png, Clipboard_2024-01-18-21-57-20.png, Clipboard_2024-01-18-22-03-31.png, Clipboard_2024-01-19-17-05-09.png, Clipboard_2024-01-19-17-08-44.png, Clipboard_2024-01-19-17-17-08.png, Clipboard_2024-01-19-17-24-56.png, Clipboard_2024-01-19-17-26-42.png, Clipboard_2024-01-19-17-28-21.png]
tags: [RAIT]
title: 04_Kalman Filters 1
created: '2024-01-19T04:52:31.464Z'
modified: '2024-01-23T02:29:44.785Z'
---

# 04_Kalman Filters 1
![](@attachment/Clipboard_2024-01-18-20-53-01.png)
x<sub>Tk + 1</sub> = x<sub>Tk</sub> + x<sub>velocity</sub> + $\Delta T$

It is an iterative process of predicting, the measuring + adjusting to do a better job at predicting the movement on the next time unit.

1. Prediction
2. Correction / Observation Update

![](@attachment/Clipboard_2024-01-18-20-58-35.png)

![](@attachment/Clipboard_2024-01-18-21-01-26.png)
This is the state transition matrix, where x and y are x and y coordinates while x<sub>dot</sub> and y<sub>dot</sub> are velocities for x and y.

![](@attachment/Clipboard_2024-01-18-21-03-33.png)
The Q matrix is supposed to include errors but it is not included in the project.

For velocity, that can be eventually implied from position measurements over time on the project.
![](@attachment/Clipboard_2024-01-18-21-16-19.png)

![](@attachment/Clipboard_2024-01-18-21-18-16.png)
The z matrix is the measurement matrix.

![](@attachment/Clipboard_2024-01-18-21-19-03.png)
This is the uncertainty matrix that can contain the covariance for x and y. You might get a numerical error if the entire matrix has 0. So you may need dummy values here initially.

![](@attachment/Clipboard_2024-01-18-21-21-56.png)
Note that variables with a hat are predictions.
K is the gain, where it is a ratio between uncertainty from the previously predicted measurement over the total uncertainty in measurement and observation.

![](@attachment/Clipboard_2024-01-18-21-28-00.png)
From this covariance equation, it indicates that with time, this amount gets reduced (subtraction).

![](@attachment/Clipboard_2024-01-18-21-30-57.png)
![](@attachment/Clipboard_2024-01-18-21-32-25.png)
![](@attachment/Clipboard_2024-01-18-21-34-43.png)
R is due to measurements.
P can be fairly large initially while others are 0 for the - terms. But if it is too large or too small, then the filter may never converge.

![](@attachment/Clipboard_2024-01-18-21-43-36.png)
The right one is for uncertainty for measurements. 

![](@attachment/Clipboard_2024-01-18-21-45-47.png)
![](@attachment/Clipboard_2024-01-18-21-48-47.png)
Innov means innovation.

![](@attachment/Clipboard_2024-01-18-21-54-02.png)
1. Green
2. Blue (measurement)
3. Red
Notice that the Gaussian bell gets more peaky (small covariance) as it gets updated. It means that the modeling is getting more accurate with time.

![](@attachment/Clipboard_2024-01-18-21-57-20.png)
x = position
x_dot = velocity
x_dotdot = acceleration
Figure out y?

![](@attachment/Clipboard_2024-01-18-22-03-31.png)
![](@attachment/Clipboard_2024-01-19-17-05-09.png)
Estimation - estimation from a set of noisy measurements
Filtering - uses all data up to current time to estimate a state

![](@attachment/Clipboard_2024-01-19-17-08-44.png)
Initializing covariance to 10k might be overkill. If you initialize this with a value that is too small, then it might have a hard time converging.

![](@attachment/Clipboard_2024-01-19-17-17-08.png)
The uncertainty that is due to your sensors (check datasheet). You can look at the $\sigma$ values perhaps in your test cases for the project. The covariance values for test cases are within a small reasonable range (_maybe pick the upper end_).

F matrix contains an exact model. Q is not necessary for the project. But if it is indeed needed, then start with acceleration first in above matrix (highest order derivative term).

![](@attachment/Clipboard_2024-01-19-17-24-56.png)
Innovation covariance is related to the uncertainty in the measurement space; predicted uncertainty in _x_ only (position, not the velocity or acceleration).

![](@attachment/Clipboard_2024-01-19-17-28-21.png)
Innovations (black dots) should fall within the covariance trace (red traces). This should improve with time.
Purple trace -> P is initialized with too small values
