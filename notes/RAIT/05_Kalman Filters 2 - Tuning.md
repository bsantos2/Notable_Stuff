---
tags: [RAIT]
title: 05_Kalman Filters 2 - Tuning
created: '2024-01-20T18:23:46.988Z'
modified: '2024-01-23T02:29:48.473Z'
---

# 05_Kalman Filters 2 - Tuning

Since KF is model based, there will be some tuning required in the project. The focus is on how to initialize P and R matrices and how R can be adaptive.

Please refer to the kf-tune and kf-adptR jupyter notebooks.

## $\sigma_r$ Tuning
- When TA reduced sigma_r, a dependency on R, from 1.5 to 1, it was noticed that it did a better job at predicting. Changing it to 0.5 takes more time to converge to the truth as the covariance is too small.

## R Tuning
- TA shows in notebook how to do get R residual and input that back  in to the filter for a more adaptable approach to tuning the filter.




