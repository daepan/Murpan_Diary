### Different Types of Parsing
- Top-Down Parsing
	- Beginning with the start symbol, try to guess the productions to apply to end up at the user's program
- Bottom-up Parsing
	- Beginning with the user's program, try to apply productions in reverse to convert the program back into the start symbol

### Parsing as a Search
- An idea : **treat parsing as a graph search**
- Each node is a **sentential form**

### Our First Top-Down Algorithm
- Breadth-First Search


### BFS is Slow
- Enormus time and memory usage
	- Lots of wasted effort
	- High branching factor

### Leftmost Derivations
- Recall: A leftmost derivation is one where we always expand the leftmost symbol first
- Updated algorithm
	- Do a breadth-first search, only considering left most derivations

### Leftmost DFS
- Idea: Use depth-first search
- advantages
	- Lower memory usage
	- High performance
	- Easy to implement

### Predictive Parsing
- The leftmost DFS/BFS algori