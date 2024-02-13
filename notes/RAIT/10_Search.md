---
attachments: [Clipboard_2024-02-10-13-22-39.png, Clipboard_2024-02-10-13-29-05.png, Clipboard_2024-02-10-13-31-26.png, Clipboard_2024-02-10-13-33-43.png, Clipboard_2024-02-10-13-38-30.png, Clipboard_2024-02-10-13-42-21.png, Clipboard_2024-02-10-13-56-35.png, Clipboard_2024-02-11-09-25-02.png, Clipboard_2024-02-11-09-33-48.png, Clipboard_2024-02-11-09-41-22.png, Clipboard_2024-02-11-09-44-43.png, Clipboard_2024-02-11-14-11-36.png, Clipboard_2024-02-11-14-12-59.png, Clipboard_2024-02-11-14-50-24.png, Clipboard_2024-02-11-15-09-11.png, Clipboard_2024-02-11-15-11-04.png, Clipboard_2024-02-11-15-12-14.png]
tags: [RAIT]
title: 10_Search
created: '2024-02-10T21:18:48.956Z'
modified: '2024-02-11T23:12:50.013Z'
---

# 10_Search

## Motion Planning
Process of finding a path from start to finish is called _planning_ or in the context of robots, it's called _robot motion planning_.
![](@attachment/Clipboard_2024-02-10-13-22-39.png)
Note that _cost_ refers to the time it takes to travers the path.

## Compute Cost for Breadth First Search
![](@attachment/Clipboard_2024-02-10-13-29-05.png)
Tricky. The answer is 7 because you need 6 moves and 1 turn for a total of 7 moves.
![](@attachment/Clipboard_2024-02-10-13-31-26.png)
In this scenario, the cost of each action is adjusted such that a move to turn left or right has a cost of 1, which is just as much as one sinlge move. 
![](@attachment/Clipboard_2024-02-10-13-33-43.png)
Now suppose a left turn is very expensive at a cost = 10, what is the cost? 15
UPS and Fedex avoid left turns altogether during rush hours because it takes a lot of time.
![](@attachment/Clipboard_2024-02-10-13-38-30.png)
### Note: if the cost for each action is all uniform at cost = 1, breadth-first search can be a suitable algorithm. Otherwise, if costs are not uniform per action, then a uniform cost search is more appropriate.

## Maze for Breadth First Search
![](@attachment/Clipboard_2024-02-10-13-42-21.png)

## Maze 2 for Breadth First Search
![](@attachment/Clipboard_2024-02-10-13-56-35.png)
Answer is 11.

## First Search Program (Uniform-Cost Search)
_Again_, since the cost is uniform, a BFS is more appropriate.
### Steps taken for code, where each triplet has (cost, x_coordinate, y_coordinate):
1. Initial postion
2. Take initial position
3. Create list of next steps (neighbors) from that node, update the cost for each action.
4. Now take an action from the list and repeat 3
5. This is done recursively until you reach the target.

## A*
Unlike BFS or uniform cost search, this algorithm uses a heuristic function that determines how much the cost is to calculate to the goal, before actually deciding what the next move is.
![](@attachment/Clipboard_2024-02-11-09-25-02.png)
On the right, each unit in the grid has a value that represents the # of steps to reach from that unit to the goal.
$h(x, y) <=$ distance to goal from x,y
_Heuristic_ function like this also gives you the best estimate of the next step to the goal.
![](@attachment/Clipboard_2024-02-11-09-33-48.png)
A* uses the function $f = g + h(x,y)$ where g is the number of steps taken thus far, and h(x,y) is the result of the heuristic function.
 
## Dynamic Programming
![](@attachment/Clipboard_2024-02-11-09-41-22.png)
![](@attachment/Clipboard_2024-02-11-09-44-43.png)
_Policy_ maps a grid cell above into an action that it thinks that is the best thing to do. For this class, _value iteration_ technique is used.
![](@attachment/Clipboard_2024-02-11-14-11-36.png)
You recursively find the f(x, y) for each cell.
![](@attachment/Clipboard_2024-02-11-14-12-59.png)
It takes 15 steps to reach the goal.

## Summary
![](@attachment/Clipboard_2024-02-11-14-50-24.png)

## Other quizzes
![](@attachment/Clipboard_2024-02-11-15-09-11.png)
Poor heuristic because 10 is larger than allowable heuristic range of 0 -> 2 for 3 x 3 square.

![](@attachment/Clipboard_2024-02-11-15-12-14.png)
It may still find a path but as you can see above, it totally missed the shortest path (of going through 100 cell) and goes through the drawn path above.




