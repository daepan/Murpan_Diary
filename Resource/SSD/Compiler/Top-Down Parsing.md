### Different Types of Parsing
- Top-Down Parsing
	- Beginning with the start symbol, try to guess the productions to apply to end up at the user's program
- Bottom-up Parsing
	- Beginning with the user's program, try to apply productions in reverse to convert the program back into the start symbol

### Parsing as a Search
- An idea : **treat parsing as a graph search**
- Each node is a **sentential form**

### Our First Top-Down Algorithm