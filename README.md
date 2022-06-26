### EX.NO: 05
### DATE: 23-05-2022
# Hill Climbing Algorithm for Eight Queens Problem
## AIM
To develop a code to solve eight queens problem using the hill-climbing algorithm.
## THEORY
Hill climbing algorithm is a local search algorithm which continuously moves in the direction of increasing elevation/value to find the peak of the mountain or best solution to the problem. It terminates when it reaches a peak value where no neighbor has a higher value.Itis a technique which is used for optimizing the mathematical problems. One of the widely discussed examples of Hill climbing algorithm is Traveling-salesman Problem in which we need to minimize the distance traveled by the salesman.It is also called greedy local search as it only looks to its good immediate neighbor state and not beyond that.Hill Climbing is mostly used when a good heuristic is available.

## DESIGN STEPS
A node of hill climbing algorithm has two components which are state and value.
### STEP 1:
Import the necessary libraries
### STEP 2:
Define the Intial State and calculate the objective function for that given state
### STEP 3:
Make a decision whether to change the state with a smaller objective function value, or stay in the current state.
### STEP 4:
Repeat the process until the total number of attacks, or the Objective function, is zero.
### STEP 5:
Display the necessary states and the time taken.
## PROGRAM
```python
#DEVELOPED BY: M.LOKESH KRISHNAA
#REFISTER NO: 212220230030
%matplotlib inline
import matplotlib.pyplot as plt
import random
import math
import sys
from collections import defaultdict, deque, Counter
from itertools import combinations
from IPython.display import display
from notebook import plot_NQueens
class Problem(object):
    def __init__(self, initial=None, goal=None, **kwds): 
        self.__dict__.update(initial=initial, goal=goal, **kwds)         
    def actions(self, state):        
        raise NotImplementedError
    def result(self, state, action): 
        raise NotImplementedError
    def is_goal(self, state):        
        return state == self.goal
    def action_cost(self, s, a, s1): 
        return 1
    def __str__(self):
        return '{0}({1}, {2})'.format(
            type(self).__name__, self.initial, self.goal)

class Node:
    def __init__(self, state, parent=None, action=None, path_cost=0):
        self.__dict__.update(state=state, parent=parent, action=action, path_cost=path_cost)
    def __len__(self): 
        return 0 if self.parent is None else (1 + len(self.parent))
    def __lt__(self, other): 
        return self.path_cost < other.path_cost
failure = Node('failure', path_cost=math.inf) # Indicates an algorithm couldn't find a solution.
cutoff  = Node('cutoff',  path_cost=math.inf) # Indicates iterative deepening search was cut off.
def expand(problem, state):
    return problem.actions(state)
class NQueensProblem(Problem):
    def __init__(self, N):
        super().__init__(initial=tuple(random.randint(0,N-1) for _ in tuple(range(N))))
        self.N = N
    def actions(self, state):
        neighbors = []
        for i in range(self.N):
            for j in range(self.N):
                if j == state[i]:
                    continue
                s1 = list(state)
                s1[i]=j
                new_state = tuple(s1)
                yield Node(state=new_state)

    def result(self, state, row):
        col = state.index(-1)
        new = list(state[:])
        new[col] = row
        return tuple(new)
    def conflicted(self, state, row, col):
        return any(self.conflict(row, col, state[c], c)
                   for c in range(col))
    def conflict(self, row1, col1, row2, col2):
        return (row1 == row2 or  # same row
                col1 == col2 or  # same column
                row1 - col1 == row2 - col2 or  # same \ diagonal
                row1 + col1 == row2 + col2)  # same / diagonal
    def goal_test(self, state):
        return not any(self.conflicted(state, state[col], col)
                       for col in range(len(state)))
    def h(self, node):
        num_conflicts = 0
        for (r1,c1) in enumerate(node.state):
            for (r2,c2) in enumerate(node.state):
                if (r1,c1)!=(r2,c2):
                    num_conflicts += self.conflict(r1,c1,r2,c2) 
        return num_conflicts
def shuffled(iterable):
    items = list(iterable)
    random.shuffle(items)
    return items
def argmin_random_tie(seq, key):
    return min(shuffled(seq), key=key)
def hill_climbing(problem,iterations = 10000):
    current = Node(problem.initial)
    i=1
    while i < iterations:
        neighbors = expand(problem,current.state)
        if not neighbors:
            break
        neighbour = argmin_random_tie(neighbors,key=lambda node:problem.h(node))
        if problem.h(neighbour)<=problem.h(current):
            current.state= neighbour.state
            if problem.goal_test(current.state)==True:
                print('The Goal state is reached at {0}'.format(i))
                return current 
        i += 1        
    return current 
nq1=NQueensProblem(8)    
plot_NQueens(nq1.initial)
n=[8,16,32,64]
time=[]
case=1
for i in n:
    nq1=NQueensProblem(i)
    n1 = Node(state=nq1.initial)
    num_conflicts = nq1.h(n1)
    print("Case {0}:\tN-value:{1}".format(case,i))
    print("Initial Conflicts = {0}".format(num_conflicts))
    import time
    start=time.time()
    sol1=hill_climbing(nq1,iterations=20000)
    end=time.time()
    print(sol1)
    sol1.state
    num_conflicts = nq1.h(sol1)
    print("Final Conflicts = {0}".format(num_conflicts))
    print("The total time required for 20000 iterations is {0:.4f} seconds\n\n".format(end-start))
    case+=1
```
## OUTPUT:
![WhatsApp Image 2022-05-23 at 8 36 28 AM](https://user-images.githubusercontent.com/75234646/169735803-f6c43a89-156c-4db9-8edc-5b6357041bd6.jpeg)
![Screenshot (220)](https://user-images.githubusercontent.com/75243072/169692105-9c8f753f-cee4-473d-9c19-7445f37213c0.png)<br>
![Screenshot (219)](https://user-images.githubusercontent.com/75243072/169692118-9adc3c75-e85a-479d-8709-11d488e32535.png)
## Solution Justification:
When the state is larger, the longer it take to complete the search.The iteration will undergo untill the objective function reaches zero.
## Time Complexity Plot
#### Plotting a graph for various value of N and time(seconds)
![Screenshot (222)](https://user-images.githubusercontent.com/75243072/169692447-ca244793-ed73-46f0-8af7-236f679532a3.png)
## RESULT:
Hence, a code to solve eight queens problem using the hill-climbing algorithm has been implemented.
