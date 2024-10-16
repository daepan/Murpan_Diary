
Lexical Analysis Scanning : identify logical pieces fo the description
Syntax Analysis Parsing : Identify how those pieces relate to each other
Semantic analysis: Identify the meaning of the overall structure
IR Genenration: Design one possible structure
IR Optimization: Simplify the intended structure
Genenration: Fabricate the structure
Optimization: Improve the resulting structure

* Source Code => Lexical Analysis,Syntax Analysis, Semantic analysis, IR Genenration : FrontEnd => IR Optimization, Genenration, Optimization: BackEnd => 

![](https://i.imgur.com/tbiZVxF.png)

## Lexical Analysis
* Some tokens can have **attributes** that store extra information about the token
* here we store which integer is represented

### Formal Languages
- A **formal language** is a set of strings

### Regular Expreessions
* Regular expressions are a family of descriptions that can be used to capture certain languages (the regular languages)

### Atomic Regular Expression
- The symbol **ε** is a regular expreession matches the empty string
- For any symbol a, the symbol **a** is a regular expression that just matches a.

### Compound Regular Expressions
- if R1 and R2 are regular expressions, R1R2 is a regular expreesion represents the **concatenation** of the languages of R1 and R2
- If R1 and R2 are regular expressions R1 | R2 is a regular expression representing the **union** of R1 and R2
- If R is regular expression, R* is a reuglar expreession for the Kleene closure of R
- If R is a regualr expression, **(R)** is a regular expression with the same meaning as R

### Implementing Regular Expressions
* Regular expressions can be implemented using **finite automata**
* There are two main kinds of finite automata
	* NFAs (nondeterministic finite automata)
	* DFAs (deterministic finite automata)
		* A DFA is like NFA, but with tighter restrictions:
			* Every state must have **exactly one** transition defined for every letter

### Whitespace Tokens
- NEWLINE marks the end of a line
- INDENT indicates an increase in indentaion
- DEDENT indicates a decrease in indentation

### Summary
- Lexical analysis splits input text into tokens holding a lexeme and an attribute
- Lexemes are sets of strings often defined with regular expression
- Regular expressions can be converted to NFAs and from there to DFAs
- Maximal-munch using an automaton allows for fast scanning
## Syntax Analysis

### What is Syntax Analysis?
- In syntax analysis (or parsing), we wnat to interpret what those tokens mean.

### Formal Languages
- An alphabet is a set Σ of symbols that act as letters
- A language over Σ is a set of strings made from symbols in Σ

### Context-Free Grammars
- A context-free grammar is a formalism for defining languages.
- Can define the context-free languages, a strict superset of the the regular languages.

### Derivations
- This sequence of steps is called a derivatino

### Leftmost derivations
- A leftmost derivation is a derivation in which exch step expands the leftmost nonterminal
- A rightmost derivation is a derivation in which each step expands the rightmost nonterminal

### Parse Trees
- A parse tree is a tree encoding the steps in a derivation

### The Goal of Parsing
- Goal of syntax analysis: Recover the structure dexcribed by a series of tokens

### Ambiguity
- A CFG is said to be **ambiguous** if there is at least one string with two or more parse trees

### Resolving Ambiguity
- If a grammar can be made unambiguous at all, it is usually made unambiguous through **layering**

### Abstract Syntax Trees
- A parse tree is a **concrete syntax tree**; it shows exactly how the text was derived
- A more useful structure is an **abstract syntax tree**, which retains only the essential structure of the input
## Semantic Analysis

## IR Generation


# Scannig a Source File


## Scannig is Hard
* C++ : Nested template declaration