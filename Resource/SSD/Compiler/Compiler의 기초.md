
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

### Goals of Lexical Analysis
- Convert from physical description of a program into sequence of of **tokens**
- Each token is associated with a **lexeme**
- Each token may have optional **attrubutes**
외울 것
tokens
lexeme
attributes
### Formal Languages
- A **formal language** is a set of strings

### Regular Expreessions
* Regular expressions are a family of descriptions that can be used to capture certain languages (the regular languages)

### Atomic Regular Expression
- The symbol **ε** is a regular expreession matches the empty string
- For any symbol a, the symbol **a** is a regular expression that just matches a.

### Compound Regular Expressions
- if R1 and R2 are regular expressions, R1R2 is a regular expreesion represents the **concatenation** of the languages of R1 and R2
- If R1 and R2 are regular expressions **R1 | R2** is a regular expression representing the **union** of R1 and R2
- If R is regular expression, R* is a reuglar expreession for the Kleene closure of R
- If R is a regualr expression, **(R)** is a regular expression with the same meaning as R

외울 것
concatenation
union
kleene closure
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

외울 것 
lexemes
regular
maximal-munch
## Syntax Analysis

### What is Syntax Analysis?
- In syntax analysis (or parsing), we wnat to interpret what those tokens mean.

### Formal Languages
- An alphabet is a set Σ of symbols that act as letters
- A language over Σ is a set of strings made from symbols in Σ

### Context-Free Grammars
- A context-free grammar is a formalism for defining languages.
- Can define the context-free languages, a strict superset of the the regular languages.
외울 것
context-free grammer
### Derivations
- This sequence of steps is called a derivations
### Leftmost derivations
- A leftmost derivation is a derivation in which exch step expands the leftmost nonterminal
- A rightmost derivation is a derivation in which each step expands the rightmost nonterminal

### Parse Trees
- A parse tree is a tree encoding the steps in a derivation
외울 것 
derivation
### The Goal of Parsing
- Goal of syntax analysis: Recover the structure described by a series of tokens
외울 것
structure
### Ambiguity
- A CFG is said to be **ambiguous** if there is at least one string with two or more parse trees
외울 것
ambiguous
### Resolving Ambiguity
- If a grammar can be made unambiguous at all, it is usually made unambiguous through **layering**
외울 것
layering
### Abstract Syntax Trees
- A parse tree is a **concrete syntax tree**; it shows exactly how the text was derived
- A more useful structure is an **abstract syntax tree**, which retains only the essential structure of the input
외울 것 
abstract
## Semantic Analysis

### Implementing Semantic Analysis
- Attribute Grammars
	- Augment bison rules to do checking durgin parsing
- Recursive AST Walk

### Challenges in Semantic Analysis  
A. Reject the largest number of ( incorrect ) programs.
B. Accept the largest number of ( correct ) programs. 
C. Do so ( quickly ).

### Other Goals of Semantic Analysis
Gather useful information about program for later phases:

1. Determine what variables are meant by each ( identifier ).
    
2. Build an internal ( representation ) of inheritance hierarchies.
    
3. Count how many variables are in ( scope ) at each point.
    

### Implementing Semantic Analysis

( Attribute ) Grammars

1. Augment bison rules to do checking during parsing.
    
2. Approach suggested in the Compilers book.
    
3. Has its limitations; more on that later.
    

( Recursive ) AST Walk

1. Construct the AST, then use virtual functions and recursion to explore the tree.
    
2. The approach we'll take in this class.
    

### Two kinds of checking in Sematic Analysis
( Scope )-Checking

1. How can we tell what object a particular identifier refers to?
    
2. How do we store this information?
    

( Type )-Checking

1. How can we tell whether expressions have valid types?
    
2. How do we know all function calls have valid arguments?
    

<Scope & Symbol Tables>

A. The scope of an entity is the set of ( locations ) in a program where that entity's name refers to that entity.

B. A symbol table is a ( mapping ) from a name to the thing that name

refers to.

### Spaghetti Stacks 
A. Treat the symbol table as a ( linked ) structure of scopes.  
B. Each scope stores a pointer to its ( parents ), but not vice-versa.  
C. From any point in the program, symbol table appears to be a ( stack ).

## IR Generation


# Scannig a Source File


## Scannig is Hard
* C++ : Nested template declaration