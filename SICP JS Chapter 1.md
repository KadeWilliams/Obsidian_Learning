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
A critical aspect of a programming language is the means it provides for using names to refer to computational objects, and our first such means are _constants_.

We say that the name identifies a constant whose _value_ is the object. 

In JavaScript, we name constants with _constant declarations_.
```js
const size = 2;
```
This causes the interpreter to associate the value 2 with the name `size`. Once the name `size` has been associated with the number 2, we can refer to the value 2 by name: 
```js
size;
```
Constant declaration is our language's simplest means of abstraction, for it allows us to use simple names to refer to the results of compound operations. 

It should be clear that the possibility of associating values with names and later retrieving them means that the interpreter must maintain some sort of memory that keeps track of the name-object pairs. This memory is called the _environment_ (more precisely the _program environment_ (**execution context**)).

# 1.1.3 Evaluating Operator Combinations
In evaluating operator combinations, the interpreter is itself following a procedure. 

To evaluate an operator combination, do the following: 
1. Evaluate the operand expressions of the combination.
  - This step dictates that in order to accomplish the evaluation process for a combination we must first perform the evaluation process on each operand of the combination  
3. Apply the funciton that is denoted by the operator to the arguments that are the values of the operands. 

The evaluation rule is _recursive_ in nature; that is, it includes, as one of its steps, the need to invoke the rule itself. 

Viewing evaluation in terms of the tree, we can imagine that the values of the operands percolate upward, starting from the terminal nodes and then combining at higher and higher levels. 

_Tree accumulation_ - general kind of process; "percolate values upward" form of evaluation rule 

The key point to notice when combining primitives is the role of the environment (execution context) in determining the meaning of the names in expressions. It is meaningless to speak of the value of an expression such as `x + 1` without specifying any information about the environment that would provide a meaning for the name `x`. The general notion of the environment as providing a context in which evaluation takes place will play an important role in our understanding of program execution. 

# 1.1.4 Compound Functions 
_Function Declarations_ - a much more powerful abstraction technique by which a compound operation can be given a name and then referred to as a unit.

The simplest form of a function declaration is 

function _name_(_parameters_) { return _expression_;} 

The _name_ is a symbol to be associated with the function definition in the environment. The _parameters_ are the names used within the body of the function to refer to the corresponding arguments of the function. In the simplest form, the _body_ of a function declaration is a single _return statement_, which will yield the value of the function application, when the parameters are replaced by the actual arguments to which the function is applied. 

After declaring a function we are then able to use it in a _function application_ expression. 

Function applications are - after operator combinations - the second kind of combination of expressions into larger expressions that we encounter. The general form of a function application is

_function-expression(argument-expressions)_ (executing the function)
- where the _function-expression_ of the application specifies the function to be applied to the comma-separated _argument-expressions_. To evaluate a function application, the interpreter follows a procedure quite similar to the procedure for operator combinations:
  - To evaluate a function application:
    - Evaluate the subexpressions of the application, namely the function expression and the argument expressions
    - Apply the function that is the value of the function expression to the values of the argument expressions.

In addition to compound functions, any JavaScript environment provides primitive functions that are built into the intepreter or loaded from libraries. 

# 1.1.5 The Substitution Model for Function Application
The interpreter evaluates the elements of the application and applies the function to the arguments. 



