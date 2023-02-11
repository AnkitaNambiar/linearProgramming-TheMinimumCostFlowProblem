# linearProgramming-TheMinimumCostFlowProblem
Solved for a class exercise in the course 'Optimization And Simulation Methods.' 

# The Minimum Cost Flow Problem

# Problem:
Given a network, with a cost c_ij , upper bound u_ij , and lower bound 0 associated with each directed path (i, j), and supply of, or demand for, b(i) units of some commodity at each node. Minimize the cost flow of the network.

### Problem Restated
In simpler terms, we are given a network for the flow of some units of a commodity. There are set paths (i, j), each with a cost (c_ij) for taking the path. The paths also have an upper limit (u_ij) for the amount of commodity that can be taken through. Furthermore, the network has nodes, which can be thought of as trading ports. Each node, has a demand or supply for b(i) units of some commodity. Satisfying these conditions, minimize the cost flow of the network.

### Network Image

<img width="310" alt="Screenshot 2023-02-09 at 6 15 04 PM" src="https://user-images.githubusercontent.com/105748980/218240263-393e50c6-12a3-40e1-b0b8-8889d87dda94.png">

The arrows indicate possible paths in the network. The numbers on each arrow represent the cost of taking the path. The circles with numbers represent the Node values. The values outside of the circles represent the units of some commodity that the Node will take when it is passed through.

# Solution (Preperation for Linear-Programming)

### Variable Creation
Each variable is the path (i, j) between 2 nodes (visualized by an arrow). 
Ex: x1 is the path from Node 1 to Node 2.

x1 = (1,2)
x2 = (1,3)
x3 = (2,3)
x4 = (3,4)
x5 = (3,5)
x6 = (4,5)
x7 = (4,1)
x8 = (5,2) 

### Objective Function Creation
The objective function will find the minimized cost of using the above paths. In the equation, each path is multiplied by the corresponding cost, as seen in the network image. For example, Path x1 (1,2) has a cost of 2, so it's cost is represented by 2x1.

Minimize, 2*x1 + 5*x2 + 3*x3 + 1*x4 + 2*x5 + 2*x6 + 0*x7 + 4*x8

### Constraint Creation
Set constraints. Make sure the movement of commodities meet the Node Limits. 

Node 1 Example:
At Node 1, 5 goods are kept. To meet the limit, check the paths that lead into and out of Node 1. 
Paths moving into Node 1: Path from Node 4 to Node 1 (x7)
Paths moving out of Node 1: Path from Node 1 to 3 (x2), Path from Node 1 to 2 (x1)
Since Node 1 takes 5 goods, the total goods at Node 1 is: x7 - x1 - x2 - 5 = 0, as written below.

Total Constraints
Node 1: x7 - x1 - x2 = 5
Node 2: x1 + x8 - x3 = -10
Node 3: x3 + x2 - x4 - x5 = 0
Node 4: x4 - x7 - x6 = 2
Node 5: x5 + x6 - x8 = 3
Upper Goods Limit for Path 5: x5 <= 1

In Julia, I used linear programming with the above equations and got the output:

<img width="205" alt="Screenshot 2023-02-10 at 8 43 15 PM" src="https://user-images.githubusercontent.com/105748980/218240454-d2c05f8a-489b-4370-b96c-7bb184116ce7.png">

# Solution Explained

The solution shows us that the path usage at the equilibrium state where the minimimum cost flow occurs. The minimum cost for the network's flow is 45. The minimized flow, only uses paths x3, x4, x5, x6, and x7. 

The values for each variable represent the number of times the path is used in the flow:
x3 = (2,3) = 10 times
x4 = (3,4) = 9 times
x5 = (3,5) = 1 time
x6 = (4,5) = 2 times
x7 = (4,1) = 5 times

The objective value for the minimized cost equals the sum of the cost of using paths x3, x4, x5, x6, and x7. 
Objective function: 
2*x1 + 5*x2 + 3*x3 + 1*x4 + 2*x5 + 2*x6 + 0*x7 + 4*x8 --> 2(0) + 5(0) + 3(10) + 1(9) + 2(10) + 2(2) + 0(5) + 4(0) = 45

