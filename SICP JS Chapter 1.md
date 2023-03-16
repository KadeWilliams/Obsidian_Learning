# 1.0 Building Abstractions with Functions
*Computational processes* are abstract beings that inhabit computers. As they evolve, processes manipulate other abstract things called *data*. The evolution of a process is directed by a pattern of rules called a *program*.

A computational process, in a correctly working computer, executes programs precisely and accurately. 

## Programming in JavaScript
A JavaScript *interpreter* is a machine that carries out processes described in the JavaScript language. JavaScript inherited its most fundamental design principles, such as lexically scoped first-class functions and dynamic typing. 

# 1.1 The Elements of Programming 
Every powerful language has three mechanisms for accomplishing combining simple ideas to form more complex ideas. 
- **Primitive expressions**, which represent the simplest entities the language is concerned with
- **Means of combination**, by which compound element 
- **Means of abstraction**, by which compound elements can be named and manipulated as units 

In programming, we deal with two kinds of elements: functions and data.

Data is "stuff" that we want to manipulate and functions are descriptions of the rules for manipulating the data. 

# 1.1.1 Expressions
You type a _statement_, and the interpreter responds by displaying the result of its _evaluating_ that statement 

One kind of primitive expression is a number. (More precisely, the expression that you type consists of the numerals that represent the number in base 10). 

Expressions representing numbers may be combined with operators (such as + or \*) to form a compound expression that represents the application of a corresponding primitive function to those numbers. 

**Combinations** - Expressions which contain other expressions as components. They are formed by an _operator_ symbol in the middle, and _operand_ expressions to the left and right of it, are called _operator combinations_. The value of an operator combination is obtained by applying the function specified by the operator to the arguments that are the values of the operands. 

_Infix notation_ - The convention of placing the operator between the operands 

Even with complex expressions, the interpreter always operates in the same basic cycle: It reads the statement typed by the user, evaluates the statement, and prints the result. This mode of operation is often expressed by saying that the interpreter runs in a _read-evaluate-print loop_. 

# 1.1.2 Naming and the Environment
A criticakl aspect 
