### EXP NO: 02

### DATE:28/04/2022 


# <p align = "center"> Breadth First Search </p>

## AIM

To develop an algorithm to find the route from the source to the destination point using breadth-first search.

## THEORY
Breadth First Search (BFS) Algorithm. Breadth first search is a graph traversal algorithm that starts traversing the graph from root node and explores all the neighbouring nodes. Then, it selects the nearest node and explore all the unexplored nodes. The algorithm follows the same process for each of the nearest node until it finds the goal.It follows FIFO.It travel from left to right.

## DESIGN STEPS

### STEP 1:
Identify a location in the google map:

### STEP 2:
Select a specific number of nodes with distance and set that specific loation alone. 

### STEP 3:
Import the required packages and create some methods to define and understand breadthfirstsearch.

### STEP 4:
Include the nodes(locations) and values(distance) in dictionary as keys and values.

### STEP 5:
Pass the required location it will return the distance and destination.



## ROUTE MAP

#### Example map
![map](https://user-images.githubusercontent.com/75243072/166133888-34c37b9c-e8ed-4f27-9af6-6d633ff6a3d5.png)



## PROGRAM
```
Developed By
Student name : Kumaran.B
Reg.no : 212220230026
```
```python
%matplotlib inline
import matplotlib.pyplot as plt
import random
import math
import sys
from collections import defaultdict, deque, Counter
from itertools import combinations
class Problem(object):
    """The abstract class for a formal problem. A new domain subclasses this,
    overriding `actions` and `results`, and perhaps other methods.
    The default heuristic is 0 and the default action cost is 1 for all states.
    When yiou create an instance of a subclass, specify `initial`, and `goal` states 
    (or give an `is_goal` method) and perhaps other keyword args for the subclass."""

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
    "A Node in a search tree."
    def __init__(self, state, parent=None, action=None, path_cost=0):
        self.__dict__.update(state=state, parent=parent, action=action, path_cost=path_cost)

def __str__(self): 
        return '<{0}>'.format(self.state)
def __len__(self): 
        return 0 if self.parent is None else (1 + len(self.parent))
def __lt__(self, other): 
        return self.path_cost < other.path_cost
        
failure = Node('failure', path_cost=math.inf) # Indicates an algorithm couldn't find a solution.
cutoff  = Node('cutoff',  path_cost=math.inf) # Indicates iterative deepening search was cut off.
def expand(problem, node):
    "Expand a node, generating the children nodes."
    s = node.state
    for action in problem.actions(s):
        s1 = problem.result(s, action)
        cost = node.path_cost + problem.action_cost(s, action, s1)
        yield Node(s1, node, action, cost)
        
def path_actions(node):
    "The sequence of actions to get to this node."
    if node.parent is None:
        return []  
    return path_actions(node.parent) + [node.action]

def path_states(node):
    "The sequence of states to get to this node."
    if node in (cutoff, failure, None): 
        return []
    return path_states(node.parent) + [node.state]
FIFOQueue = deque
def breadth_first_search(problem):
    "Search shallowest nodes in the search tree first."
    node = Node(problem.initial)
    if problem.is_goal(problem.initial):
        return node
    # Remove the following comments to initialize the data structure
    frontier = FIFOQueue([node])
    reached = {problem.initial}
    while frontier:
        node = frontier.pop()
        for child in expand(problem, node):
            s = child.state
            if problem.is_goal(s):
                return child
            if s not in reached:
                reached.add(s)
                frontier.appendleft(child)
    return failure
class RouteProblem(Problem):
    """A problem to find a route between locations on a `Map`.
    Create a problem with RouteProblem(start, goal, map=Map(...)}).
    States are the vertexes in the Map graph; actions are destination states."""
    
    def actions(self, state): 
        """The places neighboring `state`."""
        return self.map.neighbors[state]
    
    def result(self, state, action):
        """Go to the `action` place, if the map says that is possible."""
        return action if action in self.map.neighbors[state] else state
    
    def action_cost(self, s, action, s1):
        """The distance (cost) to go from s to s1."""
        return self.map.distances[s, s1]
    
    def h(self, node):
        "Straight-line distance between state and the goal."
        locs = self.map.locations
        return straight_line_distance(locs[node.state], locs[self.goal])
 class Map:
    """A map of places in a 2D world: a graph with vertexes and links between them. 
    In `Map(links, locations)`, `links` can be either [(v1, v2)...] pairs, 
    or a {(v1, v2): distance...} dict. Optional `locations` can be {v1: (x, y)} 
    If `directed=False` then for every (v1, v2) link, we add a (v2, v1) link."""

    def __init__(self, links, locations=None, directed=False):
        if not hasattr(links, 'items'): # Distances are 1 by default
            links = {link: 1 for link in links}
        if not directed:
            for (v1, v2) in list(links):
                links[v2, v1] = links[v1, v2]
        self.distances = links
        self.neighbors = multimap(links)
        self.locations = locations or defaultdict(lambda: (0, 0))
    
def multimap(pairs) -> dict:
    "Given (key, val) pairs, make a dict of {key: [val,...]}."
    result = defaultdict(list)
    for key, val in pairs:
        result[key].append(val)
    return result
# Create your own map and define the nodes

chennai_to_arakkonam = Map(
    {('chennai', 'annanagar'):  8, ('annanagar', 'koyambedu'): 8, ('koyambedu', 'poonamalle'): 13, ('poonamalle', 'chettipedu'): 10, ('chettipedu', 'sengadu'): 4, ('sengadu', 'perambakkam'): 13,('perambakkam', 'thakkolam'): 13, ('thakkolam', 'arakkonam'):  12,
     ('chennai', 'perambur'): 7, ('perambur', 'ambattur'): 15, ('ambattur', 'avadi'): 10, ('avadi', 'thiruvallur'): 13,('thiruvallur', 'thiruvalangadu'): 20,('thiruvalangadu', 'vyasapuaram'): 8,('vyasapuaram', 'arakkonam'): 9,
     ('poonamalle', 'thirumalizai'): 5, ('thirumalizai', 'perumalpattu'):  10, ('perumalpattu', 'manavalanagar'): 14, ('manavalanagar', 'thiruvallur'): 3,
    ('manavalanagar', 'manavur'): 42,('manavur', 'arakkonam'): 21,('manavalanagar', 'sengadu'): 26,
    ('perumalpattu', 'nemam'): 7,('nemam', 'chettipedu'): 15,})

r0 = RouteProblem('chennai', 'arakkonam', map=chennai_to_arakkonam)
goal_state_path=breadth_first_search(r0)
print("GoalStateWithPath:{0}".format(goal_state_path))
print(path_states(goal_state_path)) 
print("Total Distance={0} Kilometers".format(goal_state_path.path_cost))

r1 = RouteProblem('chennai', 'thiruvallur', map=chennai_to_arakkonam)
goal_state_path=breadth_first_search(r1)
print("GoalStateWithPath:{0}".format(goal_state_path))
print(path_states(goal_state_path))  
print("Total Distance={0} Kilometers".format(goal_state_path.path_cost))

r2 = RouteProblem('poonamalle', 'arakkonam', map=chennai_to_arakkonam)
goal_state_path=breadth_first_search(r2)
print("GoalStateWithPath:{0}".format(goal_state_path))
print(path_states(goal_state_path))  
print("Total Distance={0} Kilometers".format(goal_state_path.path_cost))

r3 = RouteProblem('arakkonam', 'ambattur', map=chennai_to_arakkonam)
goal_state_path=breadth_first_search(r3)
print("GoalStateWithPath:{0}".format(goal_state_path))
print(path_states(goal_state_path)) 
print("Total Distance={0} Kilometers".format(goal_state_path.path_cost))

r4 = RouteProblem('perambakkam', 'avadi', map=chennai_to_arakkonam)
goal_state_path=breadth_first_search(r4)
print("GoalStateWithPath:{0}".format(goal_state_path))
print(path_states(goal_state_path)) 
print("Total Distance={0} Kilometers".format(goal_state_path.path_cost))
```


## <br>OUTPUT:
![Screenshot (238)](https://user-images.githubusercontent.com/75243072/175768659-e6164a60-9485-4168-a424-a65dd2829d7e.png)
![Screenshot (145)](https://user-images.githubusercontent.com/75243072/166918084-5b1cfede-e236-426f-9533-9c34cab94450.png)



## SOLUTION JUSTIFICATION:
The Route solutions are found by Breadth First Search algorithm(following FIFO and routes travelling from left to right).

## RESULT:
Thus,an algorithm developed to find the route from the source to the destination point using breadth-first search.

