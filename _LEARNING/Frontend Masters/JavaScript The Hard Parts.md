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

# Asynchronous JavaScript
## Promises, Async & the Event Loop
- Promises are the most significant ES6 feature
- Asynchronicity: the feature making dynamic web applications possible 
- The event loop: JavaScript's Triage
- Microtask queue, Callback queue and Web Browser features (APIs)

## Asynchronicity is the backbone of modern web development
JavaScript is:
- Single Threaded (one command runs at a time) 
- Synchronously executed (each line is run in order the code appears) 

So what if we have a task: 
- Accessing Twitter's server to get new tweets that takes a long time
- Code we want to run using those tweets

**Challenge**: We want to wait for the tweets to be stored in tweets so that they're there to run displayTweets on - but no code can run in the meantime 

## JavaScript is not enough - We need new pieces (some aren't JavaScript at all)
Our core JavaScript engine has 3 main parts
- Thread of Execution
- Memory/variable environment
- Call stack

We need to add some new components
- Web Browser APIs/Node background APIs
- Promises
- Event loop, callback/task queue and micro task queue

What is the rule for when in the queue that's been thrown out by using a "facade" function (setTimeout()) in the browser that it's then allowed out of the callback queue and into the call stack? 
- Anything in the call stack has to be gone.
- The global execution context has to be completed. All synchronous code has to be done before the callback can be executed. 

### Event loop
checks whether synchronous lines of code are still being executed or need to be executed **and** whether there are additional functions in the call stack needing to complete execution. after both checks are complete the callback queue functions are allowed onto the call stack

## ES5 Web Browser APIs with callback functions
**Problems**
- Out response data is only avaialable in the callback function - Callback hell
- Maybe it feels a little odd to think of passing a function _into_ another function only for it to run much later

**Benefits**
- Super explicit once you understand how it works under-the-hood

## ES6+ Solution (Promises)
Using two pronged "facade" functions that both: 
- Initiate background web browser work and
- Return a placeholder object (promise) immediately in JavaScript

## _then_ method and functionality to call on completion
- any code we want to run on the returned data must also be saved on the promise object 
- added using .then() method to the hidden property 'onFulfillment'
- Promise objects will automatically trigger the attached function to run (with its input being the returned data) 

## Promises 
**Problems**
- 99% of devs have no idea how they're working on under the hood
- Dubugging becomes super-hard as a result
- Developers fail technical interviews

**Benefits**
- Cleaner readable style with pseudo-synchronous style code 
- Nice error handling process

## We have rules for the execution of our asynchronously delayed code
Hold promise-deferred function in a microtask queue and callback function in a task queue (Callback queue) when the Web Browser Feature (API) finishes 
- Add the function to the Call stack (i.e. run the function) when:
	- Call stack is empty & all global code run (Have the Event Loop check this condition)
Prioritize functions in the microtask queue over the Callback queue 

## Promises, Web APIs, the Callback & Microtask Queues and Event loop enable:
- **Non-blocking applications**: This means we don't have to wait in a single thread and don't block further code from running
- **However long it takes**: We cannot predict when our Browser feature's work will finish so we let JS handle _automatically_ running the function on its completion
- **Web applications**: Asynchronous JavaScript is the backbone of the modern web..letting us build fast 'non-blocking' applications
# Classes & Prototypes (OOP)

## Classes, Prototypes - Object Oriented JavaScript
- An enormously popular paradigm for structuring our complex code
  - Has been the way we handle having 100,000+ lines of code in one application. Having some organizing structure so it's not just "do this, then this, then this etc." 
- Prototype chain - the feature behind-the-scenes that enables emulation of OOP but is a compelling tool in itself  
- Understanding the difference between __proto__ and prototype
- The new and class keywords as tools to automate our object & method creation 

## Core of development (and running code)
1. Save data (e.g. in a quiz game the scores of user1 and user2)
2. Run code (functions) using the data (e.g. increase 2's score)

Easy! So why is development hard?

In a quiz game I need to save lots of users, but also admins, quiz questions, quiz outcomes, league tables - all have data and associated functionality

In 100,000 lines of code
- Where is the functionality when I need it? 
- How do I make sure the functionality is only used on the right data? 

## That is, I want my code to be: 
1. Easy to reason about
but ALSO
2. Easy to add features to (new functionality)
3. Nevertheless efficient and performant

The Object-oriented paradigm aim is to let us achieve these three goals! 

## So if I'm storing each user in my app with their respective data (let's simplify)
user1: 
- name: 'Tim'
- score: 3

user2:
- name: 'Stephanie'
- score: 5

And the functionality I need to have for each user 
- increment functionality (there'd be a ton of functions in practice)

How could I store my data and functionality together in one place?

## Objects - store functions with their associated data! 
This is the principle of encapsulation - and it's going to transform how we can 'reason about' our code
- Protect and bundle up in one place:
  - Functionality
  - Data it applies to  

## Creating user2 using dot notation
Declare an empty object and add properties with dot notation 
```js
const user2 = {}

user2.name = "Tim"
user2.score = 6
user2.increment = function() {
  user2.score++;
}; 
```
## Creating user3 using Object.create
Object.create is going to give us fine-grained control over our object later one
```js
const user3 = Object.create(null);

user3.name = "Eva"
user3.score = 9
user3.increment = function() {
  user3.score++;
}
```
BUT THIS BREAKS THE DRY PRICIPLE 

## Solution 1. Generate objects using a function
```js
function userCreator(name, score) {
  const newUser = {};
  newUser.name = name;
  newUser.score = score;
  newUser.increment = function() {
    newUser.score++;
  };
  return newUser;
};

const user1 = userCreator("Will", 3);
const user2 = userCreator("Tim", 5);
user1.increment();
```
- **Problems**: Each time we create a new user we make space in our computer's memory for all our data and functions. But our functions are just copies.
- **Benefits**: It's simple and easy to reason about! 


## Solution 2: Using the prototype chain
**Problems**: No problems! It's beautiful. Maybe a little long-winded

Write this every single time - but it's 6 words!

```js
const newUser = Object.create(userFunctionStore);
...
return newUser;
```
**Benefits**: Super sophisticated but not standard
**this** -> implicit parameter whatever is calling the method 'this' is bound to that 

## What if we want to confirm our user1 has the property score
We can use the hasOwnProperty method - but where is it? Is it on user1? 

all objects have a __proto__ property by default which defaults to linking to a big object - Object.prototype full of (somewhat) useful functions

We get access to it via userFunctionStore's __proto__ property - the chain 

Every other language:
- while you're inside the method, the pertinent object, the object we really care about doing stuff with the data, is throughout, going to be the object we're running the method on, even if we declare other functions inside  

The `this` assignment is lexically scoped 

We don't want to use arrow functions for our methods on objects, but for the functions inside of them that make use of this. If we define a method using an arrow function on the object we lose the ability to use `this` it becomes lexically scoped to the global() `this`

## Solution 3: Intoducing the keyword that automates the hard work: `new`

When we call the function that returns an object with **_new_** in front we automate 2 things
1. Create a new user object
2. Return the new user object 

```js
const user1 = new userCreator("Eva", 9);
const user2 = new userCreator("Tim", 5);

```

But now we need to adjust how we write the body of userCreator - how can we: 
- Refer to the auto-created object?
- Know where to put our single copies of functions?

If we use a `new` keyword, the automatically created object inside of the execution context is going to be labeled as `this`

### The new keyword automates a lot of our manual work

```js
function userCreator(name, score) {
  this.name = name;
  this.score = score;
};

userCreator.prototype.increment = function() { this.score++; };
userCreator.prototype.login = function() { console.log("login"); }; 

const user1 = new userCreator("Will", 3);

user1.increment(); 
```

"Functions are both **objects** and **functions** in JavaScript"
- Every function when it is created also has an object attached to it that allows us to create and store on properties 


**How is inheritance defined?**
- An object, objA, inherits another object, objC, if objA and objC are connected through any number of __proto__. So if objA has __proto__ equal to objB, and objB has __proto__ equal to objC, then objA inherits objB and objC, while objB inherits objC. 

**What is meant by inheritance**
- It means any inheriting object can _use_ any property of inherited object.

**What is Function.prototype?**
- It is the object whom **__proto__** of every _function object_ refers. This means every _Function object_ has access to properties of Function.prototype, since every _Function object_ inherits the Function.prototype object. This also means that if a _method_ property is added to the Function.prototype object, it would be available to all the possible _function objects_ in JavaScript. 

**_this.prototype[name] = func_;**
this refers to the _Function object_ when the 'method' is invoked from the _Function objects_ like Number, String, etc. Which means we now have a new property in the _Function object_ with name "name", and its a function 'func'.

**Benefits**:
- Faster to write. Often used in practice in professional code

**Problems**:
- 95% of developers have no idea how it works and therefore fail interviews
- We have to upper case first letter of the function so we know it requires `new` to work!

## Solution 4: The class 'syntactic sugar'
We're writing our shared methods separately from our object 'constructor' itself (off in the userCreator.prototype object)

Other languages let us do this all in one place.

```js
class UserCreator {
------------------------------>   
  constructor (name, score) {   function userCreator(name, score {
    this.name = name;             this.name = name;
    this.score = score;           this.score = score;
  }                             }
------------------------------> 
  increment() { this.score++; }      userCreator.prototype.increment = function () { this.score++; };
  login () { console.log("login"); } userCreator.prototype.login = function () { console.log("login"); };
}
------------------------------> 
const user1 = new UserCreator("Eva", 9); 
user1.increment();
```

When we declare a `class` we get a function/object combo that's returned to the new object that class creates 

The `constructor` is the `function` 

**Benefits**:
- Emerging as a new standard
- Feels more like style of other languages

**Problems**:
- 99% of developers have no idea how it works and therefore fail interviews