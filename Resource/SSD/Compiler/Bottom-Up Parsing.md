### What is Bottom-Up Parsing
- Apply productions in reverse to convert the user's program to the start symbol
- We'll be exploring four **directional, predictive** bottom-up parsing techniques
	- Directional: Scan the input from left-to-right
	- Predictive: Guess which production should be inverted

### Handles
- The handle of a parse tree T is the leftmost coplete cluster of leaf nodes

### Shift / Reduce Parsing
- The bottom-up parsers we will consider are called **shift/reduce** parsers
	- Contrast with the LL(1) **predict/match** parser
- At each point, decide whether to:
	- Move a terminal across the split( **shift** )
	- Reduce a handle ( **reduce** )

### An Important Corollary
- Consequently, shift/reduce parsing means
	- Shift
		- Move a terminal from the right to the left area
	- Reduce
		- Replace some number fo symbols at the right side of the left area

### Finding Handles
- Where do we look for handles?
	- **At the top of the stack**