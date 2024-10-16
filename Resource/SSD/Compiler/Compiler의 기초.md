
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

### Choosing Tokens

### Formal Languages
- A **formal language** is a set of strings

### Regular Expreessions
* Regular expressions are a family of descriptions that can be used to capture certain languages (the regular languages)

### Atomic Regular Expression
- The symbol **ε** is a regular expreession matches the empty string
- For any symbol a, the symbol **a** is a regular expression that just matches a.

### Compound Regular Expressions
- if R1 and R2 are regular expressions, R1R2 is a regular expreesion represents the **concatenation** of the languages of R1 

## Syntax Analysis

## Semantic Analysis

## IR Generation


# Scannig a Source File


## Scannig is Hard
* C++ : Nested template declaration