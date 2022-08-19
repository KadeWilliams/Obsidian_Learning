# Asynchronous Code

[[#Callbacks]]
[[#Promises]]


## Learning Outcomes

- [ ] What is a callback?

- [ ] What is a promise?

- [ ] What are the circumstances under which promises are better than callbacks?

- [ ] What does the  `.then()` function do?

## Callbacks

A callback function is a function passed into another function as an argument, which is then invoked inside the outer function to complete some kind of routine or action

Callbacks are simply functions that get passed into other funcitons:

``` js

let myDiv = document.querySelector('.myDiv');

myDiv.addEventListener("click", function(){

                // do something

})

```

Unfortunately using callbacks can get out of hand, especially when you need to chain several of them together in a specific order.

### Callback Hell

The cause of callback hell is when people try to write JS in a way where execution happens visually from top to bottom.

#### What are callbacks?

Callbacks are just the name of a convention for using JS functions. There isn't a special thing called a "callback" in the JS language, it's just a convention. Instead of immediately returning some result like most functions, functions that use callbacks take some time to produce a result. The word "asynchronous" just means "takes some time" or "happens in the future, not right now"

You store the code that should run after the download is complete in a function. You give the function to your async function and it will then run your callback when the original function is complete.

#### How do I fix callback hell?

Callback hell is caused by poor coding practices.

1. Keep your code shallow

                Try to abstract as much into the top level of the program as possible. Write functions at the top level and then call them within the asynchronous functions to use them

2. Modularize

                Anyone is capable of creating modules (libraries). Write small modules that each do one thing, and assemble them into other moduels that do a bigger thing. You can't get into callback hell if you don't go there."

```js

module.exports.[nameExported] = functionToExport

```

3. Handle every single error

There are different types of errors: syntax errors caused by the programmer (usually caught when you try to first run the program), runtime errors caused by the programmer (the code ran but had a bug that caused something to mess up), platform errors caused by things like invalid file permissions, hard drive failure, no network connection etc.

When dealing with callbacks you are by definition dealing with tasks that get dispatched, go off and do something in the background, and then complete successfully or abort due to failure. Any experienced developer will tell you that you can never know when these errors happen, so you have to plan on them always happening.

---

This is the most important topic to understand if you want to understand how to use node. Nearly everything in node uses callbacks. They weren't invented by node, they are just part of the JS language.

Callbacks are just functions that get executed at some later time. The key to understanding callbacks is to realize that they are used when you don't know when some async operation will complete, but you do know where the operation will complete -- the last line of the async function!

When a function gets invoked in JS, the code inside that funciton will immediately get executed. Remember, just because you define a function doesn't mean it will execute. You have to *invoke* a function for that to happen.

Generally speaking in node programs when you see a variable like `callback` or `cb` you can assume it is a function

#### You don't know JS

As soon as we introduce a single continuation in the form of a callback function, we have allowed a divergence to form between how our brains work and teh way the code will operate. Any time these two diverge we run into the inevitable fact our code becomes harder to understand, reason about, debug, and maintain

##### Sequential Brain

When we fake multitasking, such as trying to type something at the same time we're talking to a friend or family member on the phone, what we're actually most likely doing is acting as fast context switchers. In other words, we switch back and forth between two more more tasks in rapid succession, simultaneously progressing on each task in tiny, fast little chunks. We do it so fast that to the outside world it appears as if we're doing these things in parallel

This sounds very similar to that of async evented concurrency within JS

##### Doing versus Planning

Even though at an operational level our brains are async evented, we seem to plan out tasks in a sequential, synchronous way. "I need to do this, then this, then finally this."

You'll notice that this higher level thinking (planning) doesn't seem very async evented in its formulation.

When a developer writes code, they are planning out a set of actions to occur.

##### Nested/Chained Callbacks

```js

doA( function(){

                doB();

                doC( function(){

                                doD();

                } )

                doE();

} );

doF();

```

The execution order for the above block of code is as follows:

- doA

- doF

- doB

- doC

- doE

- doD

The brittle nature of manually hardcoded callbacks (even with hardcoded error handling) is often far less graceful. Once you end up specifying (aka pre-planning) all the various eventualities/paths, the code becomes so convoluted that it's hard to ever maintain or update it.

##### Trust Issues

In a basic sense, deferring control to another party, doesn't regularly cause lots of problems for programs.

But don't be fooled by its infrequency. It's one of the worst and most subtle problems about callback-driven design. It revolves around the idea that sometimes ajax(..) (i.e., the "party" you hand your callback continuation to) is not a function that you wrote, or that you directly control. Many times, it's a utility provided by some third party.

We call this "inversion of control" when you take part of your program and give over control of its execution to another third party. There's an unspoken "contract" that exists between your code and the third party utility -- a set of things you expect to be maintained.

However you go about it, these sorts of checks/normalizations are fairly common on function inputs, even with code we theoretically trust. In a crude soft of way, it's like the programming equivalent of the geopolitical principle of "Trust But Verify".

So, doesn't it stand to reason that we should do the same thing about composition of async function callbacks, not just with truly external code but even with code we know is generally "under our own control"? Of course we should.

But callbacks don't really offer anything to assist us. We have to construct all that machinery ourselves, and it often ends up being a lot of boilerplate/overhead that we repeat for every single async callback

The most troublesome callback inversion of control leading to a complete breakdown along all those trust lines.

If you have code that uses callbacks, especially but not exclusively with third-party utilities, and you're not already applying some sort of mitigation logic for all these inversion of control issues, your code has bugs in it right now even though they may not have bitten you yet. Latent bugs are still bugs.

##### Trying to Save Callbacks

Another common callback pattern is called "error-first style" (sometimes called "Node style", as it's also the convention used across nearly all Node.js APIs), where the first argument of a single callback is reserved for an error object (if any). If success, this argument will be empty/falsy (and any subsequent arguments will be the success data), but if an error result is being signaled, the first argument is set/truthy (and usually nothing else is passed):

```js

const response  = (err, data) => {

                if (err) {

                                console.error(err);

                }

                else {

                                console.log(data);

                }

}

ajax("url", response);

```

#### Review

Callbacks are the fundamental unit of asynchrony in JS. But they're not enough for the evolving landscape of Async programming as JS matures.

First, our brains plan things out in sequential, blocking, single-threaded semantic ways, but callbacks express asynchronous flow in a rather nonlinear, nonsequential way, which makes reasoning properly about such code much harder. Bad to reason about code is bad code that leads to bad bugs.

We need a way to express asynchrony in a more synchronous, sequential, blocking manner, just like our brains do.

Second, and more importantly, callbacks suffer from *inversion of control* in that they implicitly give control over to another party (often a third-party utility not in your control!) to invoke the continuation of your program. This control transfer leads us to a troubling list of trust issues, such as whether the callback is called more times than we expect.

Inventing ad hoc logic to silve these trust issues is possible, but it's more difficult than it should be, and it produces clunkier and harder to maintain code, as well as code that is likely insufficiently protected from these hazards until you get visibly bitten by the bugs.

We need a generalized solution to all of the trust issues, on that can be reused for many callbacks as we create without all the extra boilerplate overhead

We need something better than callbacks. They've served us well to this point, but the future of JS demands more sophisticated and capable async patterns. The subsequent chapters in this book will dive into those emerging evolutions.

## Promises

Essentially, a promise is an object that might produce a value at some point in the future. Here's an example:

Lets say `getData()` is a function that fetches some data from a server and returns it as an object that we can use in our code:

```js

const getData = function() {

                // do something

                return data

}

```

The issue with this example is that it takes some time to fetch the data, but unless we tell our code that, it assumes that everything in the function happens essentially instantly.

We need some way to solve this problem, and tell our code to wait until the data is done fetching to continue. Promises solve this issue. Promises allow you to do this:

```js

const myData = getData()

myData.then(function(data) {

                const pieceOfData = data['key']

})

```

### Basic Promise Usage

The `new Promise()` constructor should only be used for legacy async tasks, like usage of `setTimeout` or `XMLHttpRequest`. A new Promise is created with the `new` keyword and the promise provides `resolve` and `reject` functions to the provided callback

```js

var p = new Promise(function(resolve, reject) {

                // do an async task and then...

                if () {

                                resolve("Success!")

                } else {

                                reject("Failure!")

                }

});

p.then(function(result) {

                // do something with the result

}).catch(function() {

                // do something on an error

}).finally(function() {

                // executes regardless of success or failure

})

```

Sometimes you don't need to complete an async task within the promise -- if it's possible that an async action will be taken, however, returning a promise will be best so that you can always count on a promise coming out of a given function.

Since a promise is always returned, you can always use the `then` and `catch` methods on its return value!

#### then

All promise instances get a `then` method which allows you to react to the promise. The first `then` method callback receives the result given to it by the `resolve()` call. The `then` callback is triggered when the promise is resolved. You can also chain `then` method callbacks. Each `then` receives the result of the previous `then`'s return value.

If a promise has already resolved but `then` is called again, the callback immediately fires. If the promise is rejected and you call `then` after rejection, the callback is never called.

#### catch

The `catch` callback is executed when a promise is rejected. What you provide to the `reject` method is up to you. A frequent pattern is sending an `Error` to the `catch`

#### finally

The newly introduced `finally` callback is called regardless of success or failure

#### Promise.all

There are times when you trigger multiple async interactions by only want to respond when all of them are completed -- that's where `Promise.all` comes in. The `Promise.all` method takes an arry of promises and fires one callback once they are all resolved.

A perfect way of thinking about `Promise.all` is firing off multiple AJAX (via `fetch`) requests at one time.

```js

var request1 = fetch('uri');

var request2 = fetch('uri');

Promise.all([request1, request2]).then(function(results) {

                //both promises are done

});

```

Dealing with rejection is hard. If any promise is rejected the `catch` fires for the first rejection.

#### Promise.race

`Promise.race` is an interesting function -- instead of waiting for all promises to be resolved or rejected, `Promise.race` triggers as soon as any promise in the array is resolved or rejected.

A use case could be triggering a request to a primary source and a secondary source (in case the primary or secondary are unavailable)

### You don't know JS

The issue we want to address first is the *inversion of control*, the trust is so fragilely held and so easily lost.

What if instead of handing the continuation of our program to another party, we could expect it to return us a capability to know when its task finishes, and then our code could decide what to do next?

This paradigm is called **Promises**

#### What is a Promise?

##### Future Value

Future values can either indicate a success or a failure. It's a promise to receive something in the future.

##### Values Now and Later

To consistently handle bot now and later, we make all values later: all operations become async

##### Promise value

Because Promises encapsulate the time-dependent state -- waiting on the fulfillment or rejection of the underlying value -- from the outside, the Promise itself is time-independent, and thus Promises can be composed (combined) in predictable ways regardless of the timing or outcome underneath.

Moreover, once a Promise is resolved, it stays that way forever -- it becomes an immutable at that point -- and can then be observed as many times as necessary.

Because a Promise is externally immutable once resolved, it's now safe to pass that value around to any party and know that it cannot be modified accidentally or maliciously. This is especially true in relation to multiple parties observing the resolution of a Promise. It is not possible for one party to affect another party's ability to observe Promise resolution. Immutability may sound like an academic topic, but it's actually one of the most fundamental and important aspects of Promise design, and shouldn't be casually passed over.

That's one of the most powerful and important concepts to understand about Promises. With a fair amount of work, you could ad hoc create the same effects with nothing but ugly callback composition, but that's not really an effective strategy, especially because you have to do it over and over again.

Promises are an easily repeatable mechanism for encapsulating and composing future values

##### Completion Event