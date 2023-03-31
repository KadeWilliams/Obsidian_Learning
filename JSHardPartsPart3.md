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

