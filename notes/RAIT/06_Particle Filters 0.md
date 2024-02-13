---
attachments: [Clipboard_2024-01-22-18-31-41.png, Clipboard_2024-01-22-18-34-58.png, Clipboard_2024-01-22-18-39-57.png, Clipboard_2024-01-22-18-42-26.png, Clipboard_2024-01-22-18-44-32.png, Clipboard_2024-01-27-09-29-47.png, Clipboard_2024-01-27-09-30-43.png, Clipboard_2024-01-27-09-37-45.png, Clipboard_2024-01-27-09-40-57.png, Clipboard_2024-01-27-09-53-29.png, Clipboard_2024-01-27-09-57-56.png, Clipboard_2024-01-27-10-17-20.png, Clipboard_2024-01-27-10-42-29.png, Clipboard_2024-01-27-10-47-52.png, Clipboard_2024-01-27-10-48-52.png, Clipboard_2024-01-27-11-30-10.png, Clipboard_2024-01-27-13-51-49.png]
tags: [RAIT]
title: 06_Particle Filters 0
created: '2024-01-23T02:28:39.930Z'
modified: '2024-01-27T21:52:13.326Z'
---

# 06_Particle Filters 0

## State Space
![](@attachment/Clipboard_2024-01-22-18-31-41.png)
Remember for histogram filters, the x-axis space was divided into bins/squares, hence it's discrete.
For Kalman filters, it is continuous.

## Belief
![](@attachment/Clipboard_2024-01-22-18-34-58.png)
Remember that for KF, you can only have one bump at a time (think Gaussian bell curve is unimodal), whereas for the HF, due to possible spread in probability distribution, especially if the robot is not precise, multimodal distribution can happen.

## Efficiency
![](@attachment/Clipboard_2024-01-22-18-39-57.png)
HF - exponential -> _problematic_ because it results in much more data as you expand the dimensions of your space.
KF - quadratic -> it is represented by mean, vector and covariance matrix. This is a much _more efficient_ method since adding a dimension does not blow up data, unlike HF.

## Exact of Approximate
![](@attachment/Clipboard_2024-01-22-18-42-26.png)
Both are approximations of the same posterior distribution. For KF, think about how predictions almost never match the measurements but are close. For HF, think about how your space is discrete.

## Particle Filters
![](@attachment/Clipboard_2024-01-22-18-44-32.png)
### Advantage
- easy to program!

## Creating Particles
![](@attachment/Clipboard_2024-01-27-09-30-43.png)
Each particle contains (x, y, heading in degrees)
Assume there are 1000 particles

## Importance Weight
![](@attachment/Clipboard_2024-01-27-09-37-45.png)
Your actual robot is indigo while the particle robot is in red. The four dots are pillars from which we make measurements with reference to the actual robot (actual measurement) and predicted measurements with respect to the red particle robot. 
_Importance Weight_ is a value of how important a particle is. The higher the value, the more important it is.
![](@attachment/Clipboard_2024-01-27-09-40-57.png)
On the figure above, the larger particles represent the ones with higher importance weights, as they are more likely to be closer to the actual location of the robot. The smaller dots are less important and will die off in next iterations. _Resampling_ is the process of regenerating these particle samples with a more appropriate weight assigned to them.
### Note that in the lectures, each particle will have its noise around a particle (gaussian distribution around the point). 

## Resampling
![](@attachment/Clipboard_2024-01-27-09-53-29.png)
1. Get each particle with respective weights
2. W = sum of all weights
3. $\alpha$ = each weight is normalized to W, the sum of all weights
4. The sum of all $\alpha$ = 1
5. Pick corresponding particles that meet some alpha threshold to eliminate some that are less likely to be close to the actual location, or some other criteria. _According the quiz, it could happen that you don't pick the particle with the largest probability of the 4. Look at resampling wheel_
### Quiz
What is the probability for each particle, normalized?
![](@attachment/Clipboard_2024-01-27-09-57-56.png)
### Quiz
![](@attachment/Clipboard_2024-01-27-10-17-20.png)
N = 5, where each particle is picked independently and without replacement.
The probability for one instance that P3 won't be sampled is 1 - 0.4 = 0.6. Given N = 5, the answer is $0.6^{5}$.

## Resampling Wheel
![](@attachment/Clipboard_2024-01-27-10-42-29.png)
1. Each particle corresponds to a slice 
2. The larger the weight, the larger the slice
3. Uniformly sample from U = [1...N]
4. Say we pick w6

~~~
index = U[1... N]
beta = 0
for i = 1:N
  beta += beter + U[0... 2 * wmax] #wmax = w5 from drawing above
  while w[index] < beta:
      beta = beta - w[index] 
      index = (index + 1) % N

  select p[index]
~~~

## Orientation
![](@attachment/Clipboard_2024-01-27-10-47-52.png)
![](@attachment/Clipboard_2024-01-27-10-48-52.png)
Orientation matters because this affects the position of each particle and affects its movement.

## Filters
![](@attachment/Clipboard_2024-01-27-11-30-10.png)
Very similar to iterative measure + predict!

# Bicycle Model
![](@attachment/Clipboard_2024-01-27-13-51-49.png)
Where we only consider one pair of tires












