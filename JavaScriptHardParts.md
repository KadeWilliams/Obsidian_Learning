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


# Functions & Callbacks

