# a0
<h1> Part 1: Navigation </h1>

<h3> Problem Statement <h3>
<p>A certain autonomous agent likes to fly around the
house and interrupt video recordings at the most inopportune moments (Figure 1). Suppose that a house
consists of a grid of N ×M cells, the map consists of N lines (in this case, 6) and M columns
(in this case, 7). Each cell of the house is marked with one of four symbols: p
represents the agent’s current location, X represents a wall through which the
agent cannot pass, . represents open space over which the agent can fly, and @
represents your location (presumably with video recording in progress).
Your goal is to write a program that finds the shortest path between the agent
and you. The agent can move one square at a time in any of the four principal
compass directions, and the program should find the shortest distance between
the two points and then output a string of letters (L, R, D, and U for left, right,
down, and up) indicating that solution. Your program should take a single
command line argument, which is the name of the file containing the map file.

For example:
[<>djcran@silo ~] python3 route_pichu.sh map1.txt
Shhhh... quiet while I navigate!
Here’s the solution I found:
16 UUURRDDDRRUURRDD

<p>

<h3> Abstraction </h3>

<h5> Valid states </h5>
Valid state is placing the pichu into the adjacent valid location on the map, here valid loactions are considered as '.@'.
In code we check whether the state is valid or not in moves() function using (map[move[0]][move[1]] in ".@")

<h5> Successor function </h5>
A successor function is a function that generates a next state from the current state.
In solution moves() function is a successor which generates all the possible and valid moves from current pichus position.

<h5> Cost function </h5>
A cost function gives the cost taken to reach the goal state from the initial state.
In this problem the cost is considered as uniofrm, that is for each step the pichu is traveled the cost is considered as 1.

<h5> Goal state </h5>
A goal state is a state in which a desired search is satified for what the searching algorithm is looking for.
In this problem the goal state is when the pichu reaches the '@' (our location).
and the goal state is checked using this condition 'house_map[move[0]][move[1]]=="@"'

<h5> Initial state </h5>
Initial state is a state from where the search starts from.
In this problem the initial state is present in map1,map2 text files.

<h3> Why does the program often fail to find a solution? </h3>
The code enters a infinite loop while we try to find a solution. This is because at certain point while travesing the map it kept on visiting the same node repeatedly and entering into the infinite loop. And because of this it fails to find the solution.

<h3> My approach </h3>
To exit the infinite loop and traverse the map to reach the goal state, I'm marking the visited indexes as 'v' in the house map so that it couldn't visit the same node again and travel to the goal state.
I implemented my thought by adding this statement 'house_map[move[0]][move[1]]='v''
And to find the move_count I added the current_distance independet for each state in the fringe. For each location the pichu is moved, the distance traveled is increased by 1. since the cost is considered uniform in this problem.
In the fringe each state is stored as a list of tuples, and each tuples is contains the house_map (successor states), move_count and move_string.
And to find the move string I'm adding the path along with the possible moves from the current location like if (row,col+1) is the valid possible move then pichu is moving to the right so adding 'R' to the path.
so for each state the path and the distance took to reach that state from the initial state is stored in the fringe.
If there is no possible solution, i.e. if the pichu cannot reach your location, while checking all the posible states in the fringe and pops the states from fringe and when fringe becomes empty it exits while loop ande returns -1.


<h1> Part 2: Hide-and-seek </h1>
 
<h3> Problem Statement <h3>

<p>Suppose that instead of a single agent as in Part 1, you have adopted k agents. The problem is that these
agents do not like one another, which means that they have to be positioned such that no two agents can
see one another. Write a program called arrange_pichus.py that takes the filename of a map in the same
format as Part 1 as well as a single parameter specifying the number k of agents that you have. You can
assume k ≥1. Assume two agents can see each other if they are on either the same row, column, or diagonal
of the map, and there are no walls between them. An agent can only be positioned on empty squares (marked
with .). It’s okay if agents see you, and you obscure the view between agents, as if you were a wall. Your
program should output a new version of the map, but with the agents’ locations marked with p. Note that
exactly one p will already be fixed in the input map file. If there is no solution, your program should just
display False. Here’s an example on the same sample output on the same map as in Part 1 <p>

<h3> Abstraction </h3>

<h5> State space </h5>
In the problem the state space placing the pichus in the valid locations, i.e. the location that contains '.'. Out of all the states in the state space only some states are traceable to the goal state.

<h5> Initial state </h5>
Initial state is the state where intial map is used to place pichus.

<h5> Goal state </h5>
Goal state is placing the n given pichus in all the possible and valid locations in the map so that no two pichus can see each other.

<h5> Successor function </h5>
The successor function picks all the valid states from the state space so to reach the goal state. In the problem the successor function check whether the position where the pichu is going to place is a valid location or not. If the location is '.' and no two pichus can see each other row, column or diagonally.

<h5>Cost function </h5>
In this the cost is considered uniform. like placing the pichu in location is uniform.


<h3> My approach </h3>

Initally the fringe is inserted with the initial state map. And when the successor function is called the fringe is poped and the map is passed as the argument to the successor function. Whereas the successor functon adds the pichu in to the location if that location contains '.', so at the same time I'm adding my validations so that to check whether there is any conflict in placing pichu at that particular location.
My validations include checking whether there is a pichu in the row or column or diagonal that could be seen from this particular index without any walls in between. if there is a pichu that can visible from any direction I'm returning false and moving to the next location.
I implemented the row condition in a way like if the validating location is (2,2), to validate the row I traverse first all the right columns line (2,3),(2,4) so on and then left columns in the same row like (2,1),(2,0) similarly with the columns and diagonals.
So when there is no possible locations to place all the given pichus, while checking all the states and fringe gets empty and exits the while loop and returns false.

