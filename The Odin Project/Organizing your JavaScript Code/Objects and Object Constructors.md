# Objects and Object Constructors 

There are multiple ways to define objects but in most cases, it is best to use the object literal syntax as follows:

```js
const myObject = {
	property: "Value!",
	otherProperty: 77,
	"obnoxious property": function(){
		// do stuff
	}
}
```

Get information out of an object: dot notation and bracket notation.

```js
myObject.property // "Value!"

myObject["obnoxious property"] // [Function]
```

## Learning Outcomes
- [ ] Write an object constructor and instantiate the object
- [ ] Describe what a prototype is and how it can be used
- [ ] Explain prototypical inheritance
- [ ] Understand the basic do and don'ts of prototypical inheritance
- [ ] Explain what `Object.create` does
- [ ] Explain what the `this` keyword is

## Object Constructors
When you have a specific type of object that you need to duplicate like a player or inventory items, a better way to create them is using an object constructor, which is a function that looks like this:
```js
function Player(name, marker) {
	this.name = name
	this.marker = marker
}
```

and which you use by calling the function with the keyword `new` 

```js 
const player = new Player('name', 'x')
console.log(player.name) // 'name'
```

just like with objects created using the Object Literal method, you can add functions to the object:

```js
function Player(name, marker) {
	this.name = name
	this.marker = marker
	this.sayName = function(){
		console.log(name)
	}
}
```

