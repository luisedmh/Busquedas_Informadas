# Dynamic Maze Solver - A* Search Algorithm

An advanced Artificial Intelligence project featuring a **pathfinding agent capable of navigating dynamic, unpredictable mazes** using a customized A* (A-Star) search algorithm. 

Unlike standard static pathfinding, this system operates in a changing environment where structural obstacles appear or disappear stochastically after each step taken by the agent.

---

## Key Features

* **Dynamic Environment Simulation:** Obstacles spawn (p_in = 0.5) and despawn (p_out = 0.5) dynamically, updating the map layout at each discrete step.
* **Efficient A* Re-planning:** The agent triggers real-time path re-calculation from its current position whenever the active layout changes.
* **Complex Movement Constraints:** Supports 8-way directional movement with distinct mechanical costs (Horizontal/Vertical cost: 5, Diagonal cost: 7).
* **Temporal Blockade Recovery:** Implements a proactive fallback mechanism that refreshes the environment up to 5 consecutive times if a map state becomes temporarily impossible to solve.
* **Constraint Enforcement:** Built-in safeguards to ensure dynamic obstacle density never exceeds 25% of the total maze area.

---

## Algorithm & Heuristics

The search core relies on evaluating nodes via the classic evaluation function:

f(n) = g(n) + h(n)

Where:
* g(n) is the accumulated step cost from the dynamic starting coordinate.
* h(n) is a scaled Manhattan Distance serving as an admissible heuristic:

h(n) = (|x_E - x_n| + |y_E - y_n|) * W (with W = 3)

### Execution Pipeline
1. **Plan:** Compute the optimal path on the current known layout snapshot using A*.
2. **Step:** Advance the agent exactly one node along the planned path.
3. **Mutate:** Update the grid cells independently using a uniform distribution U(0,1).
4. **Repeat:** Re-initialize A* using the new agent coordinate as the root until destination E is successfully reached.

---

## Tech Stack & Architecture

* **Language:** C++ (Standard C++11 or higher)
* **Standard Library Components:** Utilizes advanced STL containers for optimal search efficiency (e.g., custom priority queues for the Open List and hashed sets for the Closed List).
* **I/O Processing:** Custom file parser designed to ingest structured matrix-based maze instances from text files (where 3 = Start, 4 = Goal, 1 = Blocked, 0 = Free).
* **Core Metrics Tracked:**
  * Total nodes generated vs. inspected.
  * Successful steps executed.
  * Unsuccessful environment retries (up to 5 consecutive attempts).
  * Mean active obstacle ratio throughout the simulation.

---

## Installation & Setup

1. **Clone the repository:**
   git clone git@github.com-personal:luisedmh/Busquedas_Informadas.git
   cd Busquedas_Informadas

2. **Compile the source code:**
   Ensure you have a C++ compiler installed (like g++). Run the following command to compile:
   g++ -O3 src/*.cpp -o dynamic_maze_solver

3. **Run the application:**
   Execute the compiled binary:
   ./dynamic_maze_solver

4. **Custom Inputs:** Upon execution, the program will prompt you via CLI to manually input your desired (S) and (E) coordinates if you wish to override the default file locations.

---

## Sample Outputs & Visualizations

During execution, the CLI prints an interactive step-by-step rendering of the matrix maze showing the agent's progressive pathing marked with '*'.

1 1 1 1 1 1 1 1 1 1
1 * * 1 0 0 0 0 0 1
1 0 * 1 0 0 0 0 0 1
1 0 * 1 0 0 1 0 0 1
3 * * 1 1 1 1 0 0 1
1 0 0 0 0 0 0 0 0 4

*(Note: Comprehensive benchmark data evaluating search behavior across different map dimensions (M1, M2, M3) can be found compiled inside the `/docs` directory).*

---

## Skills Demonstrated
* **Advanced AI Heuristics:** Design, profiling, and comparative analysis of admissible heuristic functions.
* **Stochastic Modeling:** Implementing runtime state mutations through probabilistic variables.
* **Data Structures Performance:** High-performance indexing using Frontier (Open) and Explored (Closed) layouts to optimize node analysis.
