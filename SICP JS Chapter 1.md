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

To apply a compound function to arguments, evaluate the return expression of the function with each parameter replaced by the corresponding argument. 

_Substitution Model_ for function application can be taken as a model that determines the "meaning" of function application.
- The purpose of the substitution is to help us think about function application, not to provide a descriptioon of how the interpreter really works. Typical interpreters do not evaluate function applications by manipulating the text of a funciton to substitute values for the parameters. In practice, the "substitution" is accomplished by using a local environment for the parameters. 

## Applicative order versus normal order
In section 1.1.4, the interpreter first evaluated the function and argument expressions and then applied the resulting function to the resulting arguments. An alternative evaluation model would not evaluate the arguments until their values were needed. Instead it would first substitute argument expressions for parameters until it obtained an expression involving only operators and primitive functions, and would then perform the evaluation.  

_Normal-Order evaluation_ = "fully expand and then reduce" 
_Applicative-Order evaluation_ = "evaluate the arguments and then apply"

JavaScript uses applicative-order evaluation, partly because of the additional efficiency obtained from avoiding multiple evaluations of expressions. Because normal-order evaluation becomes much more complicated to deal with when we leave the realm of functions that can be modeled by substitution. 

# 1.1.6 Conditional Expressions and Predicates 
The general form of a conditional expression: 

_predicate ? consequent-expression : alternative-expression_

Conditional expressions begin with a _predicate_ - that is, an expression whose value is either _true_ or _false_, two distinguished _boolean_ values in JavaScript. The _predicate_ is followed by a question mark, the _consequent-expression_, a colon, and finally the _alternative-expression_.

To evaluate a conditional expression, the interpreter starts by evaluating the _predicate_ of the expression. If the _predicate_ evaluates to true, the interpreter evaluates the _consequent-expression_ and returns its value as the value of the conditional. If the _predicate_ evaluates to false, it evaluates the _alternative-expression_ and returns its value as the values of the conditional.

The word _predicate_ is used for operators and functions that return true or false, as well as for expressions that evaluate to true or false. 

In JavaScript, we express a case analysis with multipole cases by nesting conditional expressions as alternative expresions inside other conditional expressions.

The general form of a case analysis is: 
p<sub>1</sub>
? e<sub>1</sub>
: _p<sub>2</sub>_
? _e<sub>2</sub>_
...
: _p<sub>n</sub>_
? _e<sub>n</sub>_
: _final-alternative-expression_

We call a predicate together with its consequent expression a _clause_. 

Logical composition operations
- &&
  - _Logical Conjunction_ (and)  
- || 
  - _Logical Disjunction_ (or)
- ! 
  - _Logical Negation_ (not)
  - _Unary_ operator -> Takes only one argument 
   
# 1.1.7 Square Roots by Newton's Method
How does one compute square roots? The most common way is to use Newton's method of successive approximations, which says that whenever we have a guess _y_ for the value of the square root of a number _x_, we can perform a simple manipulation to get a better guess (one closer to the actual square root) by averaging _y_ with _x/y_. 

![image](https://user-images.githubusercontent.com/59847831/225989759-b4012cc1-0608-470e-8027-d5b0d126b31a.png)

The evaluation of a return expression needs to evaluate its arguments first, including the recursive call of the argument, regardless of whether the predicate evaluates to true or false. The same of course happens with the recursive call, and thus the function never actually gets applied. 

# Exercise 1.7
Key facts about floating point numbers: 
- Because each number is encoded on a finite number of bits, the number of floating point numbers that can be represented in a computer is finite
- Most of the time, a floating point number is an approximation of a real number. 
- As the size of the number represented increases, the size of the "gap" between two consecutive numbers will increase step by step
- Although there are infinitely many integers, in most programs the result of integer computations can be stored in 32 bits. In contrast, given any fixed number of bits, most calculations with real numbers will produce quantities that cannot be exactly represented using that many bits. **Therefore the result of a floating-point calculation must often be rounded in order to fit back into its finite representation.**

# Exercise 1.8
```js
function abs(x) {
    return x >= 0 ? x : - x;
}

function cube(x) {
    return x * x * x;
}

function is_good_enough(guess, x) {
    return abs(cube(guess) - x) < 0.001;
}
function div3(x, y) {
     return (x + y) / 3;
}
function improve(guess, x) {
    return div3(x / (guess * guess), 2 * guess);
}
function cube_root(guess, x) {
    return is_good_enough(guess, x)
           ? guess
           : cube_root(improve(guess, x), x);
}

cube_root(3, 27);
```

# 1.1.8 Functions as Black-Box abstractions
A program can be viewed as a cluster of functions that mirror the decomposition of the problem into subproblems. The importance of this decomposition strategy is not simply that one is dividing the program into parts. After all, we could take any large program and divide it into parts. Rather, it is crucial each function accomplishes an identifiable task that can be used as a module in defining other funcitons. We are able to regard portions of the program as "black box(es)". We are not at that moment concerned with _how_ the funciton computes its result, only with the fact _that_ it computes. 

So a function should be able to suppress detail. The users of the function may not have written the function themselves, but may have obtained it from another programmer as a black box. A user should not need to know how the function is implemented in order to use it. 

## Local Names
One detail of a function's implementation that should not matter to the user of the function is the implementer's choice of names for the function's parameters. 

A parameter of a function has a very special role in the function declaration, in that it doesn't matter what name the parameter has. Such a name is called _bound_, and we say that the function declaration _binds_ its parameters. If a name is not bound, we say that it is _free_. The set of statements for which a binding declares a name is called the _scope_ of that name. In a function declaration, the bound names declared as the parameters of the function have the body of the function as their scope. 

## Internal Declaraions and Block Structure 
Any matching pair of braces designates a _block_, and declarations inside the block are local to the block, this is called _block_ structure. 

_Lexical Scoping_ - setting the scope or range of functionality or a variable so that it may be called (referenced) from within the block of code in which it is defined

# 1.2 Functions and the Processes They Generate

