# Library-Evacuation-Agent-Based-Model
An agent based model of an emergency situation in the library of TU Delft. The model is coded in the Netlogo-environment.

1. Introduction and Problem Formulation

The TU delft needs to know how fast the library on the campus can be evacuated. In order to analyze this, we created an agent based model for the evacuation in Netlogo. In this report the model is analyzed in order to find answers to the following research questions: 
1.	What is the difference in total evacuation time if all agents exit via the main entrance versus all agents exit via the nearest exit?
2.	Systematically vary which exits all agents will exit the building through (only A, only B, only C, A or B, B or C, A or G, etc.). What are the differences in evacuation time? How many times should you run each variation?
3.	What is the difference in total evacuation time if there are signs in the building that lead the visitors to the nearest exit, compared to a building with no signs? Which assumptions do you need to make?
4.	How does gender or familiarity influence total evacuation time?
5.	Which parameter influence response time most
We seek to answer these questions by creating different scenarios for the model and running these multiple times. The results of these runs are then analyzed to answer the research questions. In order to make sure which of the results are significant, ANOVA tests are carried out for all of them.
One of the emergent patterns we expect to see is that people will follow others to the exits if they don’t know where these are located, and that people will actively take others with them towards an exit. We expect this to happen due to the constant interaction between the different agents in regard to where they are moving.
The code is located in the following files: agents.nls, evacuation.nls, floorplan.nls, routes.nls, and utilities.nls. We decided to divide the code in files to keep it comprehensible. 

2. Model Explanation

2.1 Assumptions
1.	The four tasks' visitors undertake are (in order of their time to react): wander around, study together, study alone, or look for books. We made these assumptions because it is impossible to model all different activities people do in a library apart from the most obvious ones. We regard these four activities as the most logical things to do in a library and therefore we assumed that all agents are doing one of these activities. 
2.	Employees either stock books or wander around. Since they are employed by the library, their behavior space is more limited, and we assumed that they either wander around helping/talking to visitors or are returning the books the visitors barrowed. 
3.	Only employees stop their task immediately and head to the nearest exit. Visitors (including those who are frequent visitors) take a random time to react to the alarm. Some leave immediately. We assumed that if you have no sunk costs of moving to an exit (you were already wandering around), you react without hesitation. If you are concentrated at your book, you might wait a couple of seconds before you act. That is why we assume the different reaction times depending on the tasks agents have at the time the alarm goes off. Employees have a certain responsibility to guide visitors to the exit in case of emergencies and are trained/instructed to act immediately when the alarm goes off. 
4.	Visitors are familiar with either all three exits, only exit A, or no exits. You are totally familiar with the library and know all exits because you visit here often. If the agent just came here for the first time, we assume that the agent used the main entrance (exit A) and therefor knows this exit, and since this type of agent is here for the first time, they only know this entrance/exit. The third option is that a newcomer got disorientated once inside and forget where exit A was. So, the main assumption here is that newcomers enter via the main entrance only resulting in the knowledge of only exit A or none. Exceptions are the employees and frequent visitors, who know all exits. 
5.	Visitors who are only familiar with exit A, do not listen to employees who tell them to head towards the nearest exit. We made this assumption because otherwise all visitors who initially only knew exit A, would be guided by the employees to the nearest exit the instant the alarm went off. If we kept it this way, we couldn’t experiment with the factor that some visitors only knew exit A and what that has for an effect on the evacuation time, so we made them ‘deaf’ to the guidance of the employees and frequent visitors. 
6.	Agents can communicate through walls when telling people where the nearest exit is. Moreover, visitors escape when the majority of agents (i.e. more than half of the agents) in their vicinity leaves. They also possibly count the agents on the other side of a wall. The agents they count is based on a radius. We assume that people scream and yell when the alarm goes off in a real situation, and this is our way of reflection that in our model. 
7.	The employees do not wait for all visitors to have left the building before leaving themselves. They tell others in their vicinity of the nearest exit and proceed to that exit. We just simply forgot to let them stay for a while. 
8.	Children are not necessarily accompanied by adults. They came to the library independently. Normally a supervisor or parent would immediately look for their child when a fire alarm rings, or any random adult for that matter. In our model we assumed that everybody is egocentric and aims to bring themselves to safety, as fast as they can, so no waiting around for the independent kids. 

2.2 Agents
The model contains two breeds of agents: employees and visitors. Both breeds have a male and female gender. They have the same running and walking speed: for males respectively 1 m/s and 1.5 m/s and females respectively 0.9 m/s and 1.4 m/s. Visitors who are children have a running speed of 1.05 m/s and a walking speed of 0.6 m/s. The agents are placed randomly on the map. Based on their initial location, they perform certain tasks. These will be explained hereafter.

Employees
Employees are aware of all exits. 
Employees either wander around the building or stock books. They stock books when they are in the rooms on the right side of the library. Here, they move forward with a walking speed of 0.3 m/s.
Employees do wait to see what others do when the alarm goes off; they head straight towards the nearest exit. On their way, they alert others nearby of the nearest exit.

Visitors
Visitors are either aware of no exits, all exits, or only exit A. 
Visitors either wander around aimlessly, or perform one of the following tasks:
•	Look for books
•	Study alone
•	Study together

Depending on the task they perform, their time to react to an alarm differs. When wandering around, they respond to the alarm between a randomly assigned number of 0-20 seconds. When looking for books, visitors react between 20 and 35 seconds. This choice has been made because we assumed that visitors looking for books might want to put back their books or want to continue looking for one. Visitors who are studying together take between 30 and 40 seconds to respond. Visitors who are studying together take between 20 and 25 seconds to respond to an alarm. This time is shorter than for visitors who are studying alone, because those who study together probably communicate faster amongst each other that there is a need to escape.

When the alarm goes off, visitors who do not know any exit run around aimlessly. However, when they see an employee or a visitor who knows the nearest exit, they follow them towards this exit. Visitors who only know the main exit proceed towards this exit. They do not follow others. Visitors who know all exits head towards the nearest exit.


2.3 Environment
The environment of the model is the world the agents live in. In this case it is the library of the TU Delft. In the floorplan, we gave certain areas a different color. We did this so we could assign different tasks to the visitors and the employees if they were placed there in the setup of the simulation. In these areas, which can be seen below on left, bottom and right, the visitors and the employees could perform tasks like reading books, or stocking books. Which meant that they moved slower or were not moving at all. When performing these activities their reaction time to the alarm is also different than when they were walking around the library.

Furthermore, we adjusted all the colors of the map, so that for every color there was one precise color code being used in Netlogo. So that for instance, every black patch in the floorplan was the exact same color black. This allowed us to make sure all the turtles were placed inside the building at the start of the simulation. The exits of all areas were given the color magenta, this was done as an indication for where the nodes required for the shortest path algorithm needed to be placed.


2.4 Events
When the alarm goes off (after 30 seconds), employees and visitors who know all exits head towards the nearest exit. The nearest exit is found using Dijkstra’s shortest path algorithm. The algorithm follows the steps from the Wikipedia page about Dijkstra’s algorithm:  https://en.wikipedia.org/wiki/Dijkstra%27s_algorithm.

The shortest path is from an agent to the exit is calculated in the following way:

Step 1: all nodes are marked unvisited (Visited? false).

Step 2: the distance from all nodes is set to each exit to an infinitely high number (in our case 2^30) and the initial node is set as the current node. 

Step 3: for all unvisited neighbors of the current node, the distance to them through the current node is calculated. If this calculation results in a lower number than was assigned to that node (which will always be the case with the infinite assignment at the start, and sometimes when a path between for example 3 nodes is faster than through 2 nodes) the distance to exit A, B, or C is changed to that lower number. 

Step 4: if all neighbors are checked, the current node is marked as visited so it will not be checked again. 

Step 5: In the end, nodes which have not been visited and have a distance to the exits set to infinity.  These nodes are outside of the network and will not be considered in the shortest path. 

Some employees and visitors look for a new exit every second. This is not done for every agent to prevent the simulation from running extremely slow. For these agents, each step they take, the distance between the node they are going to, and other nodes in their sight are re-evaluated. Example: a agent is moving towards node X. If there is another node in their view (node Y) with (shortest distance to exit + distance from node to agent) than node X, the agent moves towards node Y. 
