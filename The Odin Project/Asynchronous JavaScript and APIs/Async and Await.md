# Async and Await
## Introduction
Asynchronous code can become difficult to follow when it has a lot of things going on. `async` and `await` are two keywords that can help make asynchronous read more like synchronous code. This can help code look cleaner while keeping the benefits of asynchronous code. 

## Learning Outcomes
- Explaing how you declare an `async` function
- Explain what the `async` keyword does
- Explaing what the `await` keword does
- Explain what an `async` function returns 
- Explain what happens when an error is thrown inside an `async` function
- Explain how you can handle errors inside an `async` function

## The async keyword
The `async` keyword is what lets the JavaScript engine know that you are declaring an asynchronous function. This is required to use `await` inside any function. When a function is declared `async`, it automatically returns a promise; returning in an `async` function is the same as resolving a promise. Likewise, throwing an error will reject the promise. 

An important thing to understand is `async` functions are juste syntactical sugar for promises

The async keyword can also be used with any of the ways a function can be created. It is valid to use an `async` funciton anywhere you can use a normal function. 

```js
const yourAsyncFunction = async () => {
	// do something asynchronously
}

anArray.forEach(async item => {
	// do something asynchronously
})

```

## The await keyword
`await` is pretty simple: it tells JavaScriopt to wait for an asynchronous action to finish before continuing the function. It's like a 'pause until done' keyword. The `await` keyword is used to get a value from a function where you would normally use `.then()`. Instead of calling `.then()` after the asynchronous function, you would simply assign a variable to the result using `await`. Then you can use the result in your code as you would in your synchronous code. 

### Error Handling
Handling errors in `async` functions is very easy. Promises have the `.catch()` method for handling rejected promises, and since async funcitons just return a promise, you can simply call the function, and append a `.catch()` method to the end. 

```javascript
asyncFunctionCall().catch(err => {
	console.error(err)
})
```

But there's another way: the might `try/catch` block! If you want to handle the error directly inside the `async` function, you can use `try/catch` just like you would inside synchronous code. 

```js
async function getPersonsInfo(name) {
  try {
    const people = await server.getPeople();
    const person = people.find(person => { return person.name === name });
    return person;
  } catch (error) {
    // Handle the error any way you'd like
  }
}
```

Doing this can look messy, but it is a very easy way to handle errors without appending `.catch()` after your function calls. 

## JavaScript.Info: Async/Await
### Asyncs
The word "async" before a function means one simple thing: a function always returns a promise. Other values are wrapped in a resolved promise automatically. 

So, `async` ensures that the function returns a promise, and wraps non-promises in it. 

### Await
The keyword `await` makes JavaScript wait until that promise settles and returns its result. 

The function execution "pauses" and resumes when the promise settles. 

Let's emphasize: `await` literally suspends the function execution until the promise settles and then resumes it with the promise result. That doesn't cost any CPU resources, because the JavaScripot engine can do other jobs in the meantime: execute other scripts, handle events, etc. 

### Summary 
The `async` keyword before a function has two effects:
1. Makes it always return a promise
2. Allows `await` to be used in it.

The `await` keyword before a promise makes JavaScript wait until that promise settles, and then:
1. If it's an error, an exception is generated -- same as if `throw error` were called at that very place.
2. Otherwise, it returns the result.

Together they provide a great framework to write asynchronous code that is easy to both read and write.

With `async/await` we rarely need to write `promise.then/catch`, but we still shouldn't forget that they are based on promises, because sometimes we have to use these methods. Also, `Promise.all` is nice when we are waiting for many tasks simultaneously. 

