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
