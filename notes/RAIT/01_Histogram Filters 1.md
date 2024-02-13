---
attachments: [Clipboard_2024-01-13-09-30-39.png, Clipboard_2024-01-13-09-32-19.png, Clipboard_2024-01-13-09-35-07.png, Clipboard_2024-01-13-09-39-53.png, Clipboard_2024-01-13-09-52-19.png, Clipboard_2024-01-13-09-53-39.png, Clipboard_2024-01-13-09-54-49.png, Clipboard_2024-01-13-09-56-01.png, Clipboard_2024-01-13-09-58-08.png, Clipboard_2024-01-13-10-05-59.png, Clipboard_2024-01-13-10-08-09.png, Clipboard_2024-01-13-10-12-31.png, Clipboard_2024-01-13-10-14-04.png, Clipboard_2024-01-13-10-15-51.png, Clipboard_2024-01-13-10-17-48.png, Clipboard_2024-01-13-10-22-26.png, Clipboard_2024-01-13-10-22-30.png, Clipboard_2024-01-13-10-23-00.png, Clipboard_2024-01-13-10-23-16.png, Clipboard_2024-01-13-10-25-43.png, Clipboard_2024-01-13-10-27-15.png, Clipboard_2024-01-14-13-29-33.png, Clipboard_2024-01-15-08-44-43.png, Clipboard_2024-01-15-08-47-18.png, Clipboard_2024-01-15-08-48-36.png, Clipboard_2024-01-15-08-48-40.png, Clipboard_2024-01-15-08-54-19.png, Clipboard_2024-01-15-08-54-57.png]
tags: [RAIT]
title: 01_Histogram Filters 1
created: '2024-01-13T17:29:37.016Z'
modified: '2024-01-23T02:27:18.025Z'
---

# 01_Histogram Filters 1
![](@attachment/Clipboard_2024-01-13-09-30-39.png)
![](@attachment/Clipboard_2024-01-13-09-32-19.png)
![](@attachment/Clipboard_2024-01-13-09-35-07.png)
Note that there will be noise detected by the sensors. We use filters to better detection/algorithms used.
![](@attachment/Clipboard_2024-01-13-09-39-53.png)
_Collectively exhaustive_ eg. days of the week as it fills the entire state space.
_Filter_ extracts the truth from data that also has noise.
_Total probability_ is for movements.
_Bayes Rule_ is for measurements.
The aim for _localization_ is to know where the robot is and to where obstacles are (eg. a tree that we don't want to bump into).
_A belief_ is where you think an object is.
#### A measurement warrants a deltaT. A movement warrants another deltaT. If both happen at the same time, that's another deltaT.

![](@attachment/Clipboard_2024-01-13-09-52-19.png)
We collect more data, and so updates the belief for time = t + 1
![](@attachment/Clipboard_2024-01-13-09-53-39.png)
This loop continues to go on, into a recursive process, demonstrated below. This is the framework for Bayes Filter.
![](@attachment/Clipboard_2024-01-13-09-54-49.png)
Tying this up with the Bayes Filter...
![](@attachment/Clipboard_2024-01-13-09-56-01.png)

![](@attachment/Clipboard_2024-01-13-09-58-08.png)
The realizations of Bayes filter are used for _localization_ process.

## An Example

### Problem: How many clouds exist in the sky now, percentage wise? Or how many of these pixels are white, amongst blue one?
![](@attachment/Clipboard_2024-01-13-10-05-59.png)

### Possible solution: Divide and Conquer
Divide the image into 16 regions to provide an estimate
#### Algorithm 1: Naive/All or nothing
![](@attachment/Clipboard_2024-01-13-10-08-09.png)
- We count which squares have a cloud (eg. 8/16 squares)
- 50% of the sky consists of clouds
- But the clouds don't cover an entire square, though
#### Algorithm 2: Accurate Algorithm
Use a device that measures _clouds in section (%)_ on a square
![](@attachment/Clipboard_2024-01-13-10-12-31.png)
![](@attachment/Clipboard_2024-01-13-10-14-04.png)
##### Then we test our algorithm on another image
![](@attachment/Clipboard_2024-01-13-10-15-51.png)
But it's the same image! The answers are not the same because we accounted for the rectangle as one unit. The rectangle is equivalent to 4 squares!
#### Algorithm 4: Accurate and Weighted Proportions
![](@attachment/Clipboard_2024-01-13-10-17-48.png)
Fixed!
![](@attachment/Clipboard_2024-01-13-10-23-00.png)
## This algorithm is the LAW OF TOTAL PROBABILITY
![](@attachment/Clipboard_2024-01-13-10-25-43.png)
![](@attachment/Clipboard_2024-01-13-10-27-15.png)
A = percentage taken up by clouds
B = square from the cloud example

# Imperfect Movement
![](@attachment/Clipboard_2024-01-14-13-29-33.png)
This means in 1D space, there's 60% likelihood that the robot will land where it needed to go, 10% chance that it does not reach its exact destination, and 30% chance that it overshot its destination.
_The total probability is 100%_
![](@attachment/Clipboard_2024-01-15-08-44-43.png)
So we are able to take the initial belief, observe the movement (right plot), and see resulting histogram. The resulting histogram is a result of the _convolution of our initial belief with the observed imperfect movement_.
![](@attachment/Clipboard_2024-01-15-08-47-18.png)
![](@attachment/Clipboard_2024-01-15-08-48-40.png)
![](@attachment/Clipboard_2024-01-15-08-54-57.png)
The formula above is for convolving. Thus, this histogram shows the probability of where the robot will land in space, taking into consideration perfect, undershot and overshot movements.

#### After infinite number of movements, you will arrive to a uniform distribution!

#### Total probability is related to sense, whereas 





