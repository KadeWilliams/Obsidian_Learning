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
- Closure is the most esoteric of JavaScript concepts 
- Enables powerful pro-level functions like 'once' and 'memoize' 
- Many JavaScript design patterns including the module pattern use closure
- Build iterators, handle partial application and maintain state in an asynchronous world

## Functions with memories
- When our functions get called, we create a live store of data (local memory/variable environment/state) for that function's execution context
- When the function finishes executing, its local memory is deleted (except the return value)
- But what if our functions could hold on to live data between executions?
- This would let our function definitions have an associated cache/persistent memory
- But it all starts with us **returning a function from another function**

Local Memory == State 
- The live data at a particular moment in my application or function

Where you define your functions determines what data it has access to when you call it

JavaScript is ... P.L.S.R.D (Backpack)
P -> Persistent
L/S -> Lexical/Static
S -> Scope
R -> Reference
D -> Data

"Put the data in the functions _**closure**_"

Where my data is saved determines what data I have access to when it is eventually run, wherever that may be. 

```js
function outer() {
	let counter = 0;
	function incrementCounter () {counter ++;}
  return incrementCounter;
}

const myNewFunction = outer();
myNewFunction();
myNewFunction();
```

## The bond
When a function is defined, it gets a bond to the surrounding Local Memory ("Variable Environment") in which it has been defined

## The 'backpack'
- We return incrementCounter's code (function definition) out of outer into global and give it a new name - myNewFunction
- We **maintain the bond to the outer's live local memory** - it gets 'returned out' attached on the back of incrementCounter's function definition
- So outer's local memory is now stored (attached) to myNewFunction even though outer's execution context is long gone
- When we run myNewFunction in global, it will first look in its own local memory first (as we'd expect), but then in myNewFunction's 'backpack'

## What we call this 'backpack'
- Closed over 'Variable Environment' (C.O.V.E)
- Persistent Lexical Scope Referenced Data (P.L.S.R.D.)
- 'Backpack'
- 'Closure'

## Closure gives our functions persistent memories and entirely new toolkit for writing professional code
- Helper functions: Everyday professional helper functions like 'once' and 'memoize'
- Iterators and generators: Which use lexical scoping and closure to achieve the most contemporary patterns for handling data in JavaScript
- Module pattern: Preserve state for the life of an application without polluting the global namespace
- Asynchronous JavaScript: Callbacks and Promises rely on closure to persist state in an asynchronous environment

```js
function addByX(x) {
  return function(y) {
    return y + x
  }
}

function once(func) {
  let executed = false;
  let result;
  return function(num) {
    if (!executed) {
      executed = true;
      result = func(num); 
    }
    return result;
  }
}
function after(count, func) {
	return function() {
    count -= 1;
    if (count == 0) {
      return func(); 
    }
  } 
}

const called = function() { console.log('hello') };
const afterCalled = after(3, called);
afterCalled(); // => nothing is printed
afterCalled(); // => nothing is printed

function delay(func, wait) {
    setTimeout(() => {
      	return func()
    	}, wait); 
}
delay(afterCalled, 3000)

function saveOutput(func, magicWord) {
	const obj = {}
  return function(val) {
    if (val == magicWord) {
      return obj; 
    } 
    
    if (typeof val == 'number') {
      let value = func(val);
      obj[val] = value
      return val 
  	}
  }
}

// const multiplyBy2 = function(num) { return num * 2; };
// const multBy2AndLog = saveOutput(multiplyBy2, 'boo');
// console.log(multBy2AndLog(2)); // => should log 4
// console.log(multBy2AndLog(9)); // => should log 18
// console.log(multBy2AndLog('boo')); // => should log { 2: 4, 9: 18 }

// stack 
function cycleIterator(array) {
	return function() {
    let cur = array.shift(); 
    array.push(cur); 
		return cur;     
  }
}

// /*** Uncomment these to check your work! ***/
const threeDayWeekend = ['Fri', 'Sat', 'Sun'];
const getDay = cycleIterator(threeDayWeekend);
console.log(getDay()); // => should log 'Fri'
console.log(getDay()); // => should log 'Sat'
console.log(getDay()); // => should log 'Sun'
console.log(getDay()); // => should log 'Fri'

function defineFirstArg(func, arg) {
  return function(val) {
  	return func(arg, val)
  }
}

// /*** Uncomment these to check your work! ***/
const subtract = function(big, small) { return big - small; };
const subFrom20 = defineFirstArg(subtract, 20);
console.log(subFrom20(5)); // => should log 15

function defineFirstArg(func, arg) {
  return function(val) {
  	return func(arg, val)
  }
}

// /*** Uncomment these to check your work! ***/
// const subtract = function(big, small) { return big - small; };
// const subFrom20 = defineFirstArg(subtract, 20);
// console.log(subFrom20(5)); // => should log 15

//challenge 12
function censor() {
  const obj = {}; 
  return function(...args) {
    if (args[1]){
			obj[args[0]] = args[1]
    } else {
      let newStr = args[0];
      for (let key in obj) {
	      newStr = newStr.replace(key, obj[key])
      }
      return newStr
    }
    return obj
  }
}

// /*** Uncomment these to check your work! ***/
const changeScene = censor();
changeScene('dogs', 'cats');
changeScene('quick', 'slow');
console.log(changeScene('The quick, brown fox jumps over the lazy dogs.')); // => should log 'The slow, brown fox jumps over the lazy cats.'

function createSecretHolder(secret) {
	let _secret = secret;
  return {
  	getSecret() {
      return _secret;
    },
    setSecret(secret) {
      _secret = secret;
    }
  }
}

const obj = createSecretHolder(5)
console.log(obj.getSecret()); // => returns 5
obj.setSecret(2)
console.log(obj.getSecret()); // => returns 2

function callTimes() {
	let _count = 0;
  return function() {
    _count++; 
    console.log(_count);
  }
}

let myNewFunc1 = callTimes();
let myNewFunc2 = callTimes();
myNewFunc1(); // => 1
myNewFunc1(); // => 2
myNewFunc2(); // => 1
myNewFunc2(); // => 2
```

