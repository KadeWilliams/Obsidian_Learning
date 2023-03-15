https://frontendmasters.com/courses/javascript-hard-parts-v2/generalized-functions/
# JavaScript Principles
- Goes through the code line-by-line and runs/ 'executes' each line - known as the **thread of execution**
- Saves 'data' like strings and arrays so we can use that data later - in its **memory** 

When we're defining a function we are giving a function an **identifier**, which tells the computer we will use that name to call the function within our code. It's stores the identifier in memory. 

Function call -> an expression that passes control and arguments (if any) to a function and has the form: expression (expression-list{optional}) where expression is a function name or evaluates to a function address and expression-list is a list of expressions (separated by commas)

**Functions** -> code we save ('define') functions & can use (call/invoke/execute/run) later with the function's name & ()

**Execution Context** -> Created to run the code of a function - has 2 parts
- Thread of execution 
- Memory

```js
// function declaration/definition
function funcName(parameter) {
	//something happening
}

// function call
funcName('argument');
```

When a **function is declared/defined** the values passed through the definition are known as **parameters** (parameters is a placeholder)

When a **function is called** the values being passed through are known as **arguments** 

`return...` 
- locate the block of memory that is bound to the label being returned and ship it out of the local context
- we are going to evaluate a command (invocation of a function) and return a value 

**Value** refers to anything that is stored, not the label (the identifier)

There is a single thread of execution in JavaScript which means there's only one thing we can do at a time. 

**Call stack**
- JavaScript keeps track of what function is currently running 
- Run a function - add to call stack 
- Finish running the function - JS removes it from call stack
- Whatever is top of the call stack - that's the function we're currently running 

As soon as you start running JavaScript the global execution context is adding to the bottom of the call stack

When a function is called a new execution context is created. 

# Functions & Callbacks
We can generalize the function to make it reusable

## Generalizing functions 
"Parameters" (placeholders) mean we don't need to decide what data to run our functionality on until we run the function 
- Then provide an actual value ('argument') when we run the function

Higher order functions follow this same principle.
- We may not want to decide exactly what some of our functionality is until we run our function

The idea of functions is to maintain D.R.Y --> DON'T REPEAT YOURSELF

Any function, array or object, when it's saved and used anywhere else and link to it we're actually referring back to it's original saved version

### How was this possible?
Functions in javascript = first class objects

They can co-exist with and be treated like any other javascript object
1. Assigned to variables and properties of other objects 
2. Passed as arguments into functions
3. Returned as values from functions 

Higher-order functions
- Takes in a function or passes out a function
- Just a term to describe these functions - any function that does it we call that - but there's nothing different about them inherently

Callbacks and Higher Order Functions simplify our code and keep it DRY 
**Declarative readable code:** Map, filter, reduce - the most readable way to write code to work with data
**Asynchronous JavaScript:** Callbacks are a core aspect of async JavaScript, and are under-the-hood of promises, async/await

## Anonymous and Arrow Functions
- Improve immediate legibility of the code
- But at least for our purposes they are 'syntactic sugar'
- Understanding how they're working under-the-hood is vital to avoid confusion

## Pair programming 
- Tackle blocks with a partner 
- Stay focused on the problem
- Refine technical communication
- Collaborate to solve the problem 

# Closure 

