---
attachments: [Clipboard_2024-01-28-14-32-20.png, Clipboard_2024-01-28-14-33-17.png, Clipboard_2024-01-28-14-35-18.png, Clipboard_2024-01-28-14-37-35.png, Clipboard_2024-01-28-14-38-29.png, Clipboard_2024-01-28-14-45-28.png, Clipboard_2024-01-28-14-49-22.png, Clipboard_2024-01-28-15-29-27.png, Clipboard_2024-01-28-15-30-54.png, Clipboard_2024-01-28-15-31-41.png, Clipboard_2024-01-28-15-32-50.png, Clipboard_2024-01-28-15-33-21.png]
tags: [RAIT]
title: '07_Particle Filters 1: Bicycle Mode'
created: '2024-01-28T22:31:19.371Z'
modified: '2024-01-28T23:34:18.781Z'
---

# 07_Particle Filters 1: Bicycle Mode
![](@attachment/Clipboard_2024-01-28-14-32-20.png)
To simplify the model, instead of using a four wheeled vehicle, the bicycle model is used with two wheels.
![](@attachment/Clipboard_2024-01-28-14-33-17.png)
d = forward movement; distance between position of the rear wheel at time = 0 to another time
$\alpha$ = steering angle; if = 0, it means it's driving parallel to original direction, if > 0, then it's turning to the left, if < 0, then it's turning to the right
![](@attachment/Clipboard_2024-01-28-14-35-18.png)
Robots A and C, B and E have the same orientation while E and D have the same x, y positions but different orientation. 
$\theta$ = orientation angle
![](@attachment/Clipboard_2024-01-28-14-37-35.png)
The final pose of the robot is shown, where only x-axis value is affected.
![](@attachment/Clipboard_2024-01-28-14-38-29.png)
Since $\theta = 90^{0}$, then only y-axis position is affected.
![](@attachment/Clipboard_2024-01-28-14-45-28.png)
Keep in mind, _always include the original x, y positions when updating the new x, y coordinates_!
![](@attachment/Clipboard_2024-01-28-14-49-22.png)
![](@attachment/Clipboard_2024-01-28-15-31-41.png)
![](@attachment/Clipboard_2024-01-28-15-32-50.png)
![](@attachment/Clipboard_2024-01-28-15-33-21.png)
As the steering angle increases, the radius gets smaller and robot is closer to the center of the circle!
