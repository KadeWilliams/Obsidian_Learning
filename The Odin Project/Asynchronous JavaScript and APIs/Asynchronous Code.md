# Asynchronous Code

[[#Callbacks]]
[[#Promises]]


## Learning Outcomes
- [x] What is a callback? 

A callback is a function within another function. It is how older JavaScript handles asynchronous code. 

- [x] What is a promise?

A promise is a asynchronous handler function that can chain together functions and wait for resolutions. A promise is an object that might produce a value at some point in the future. 

- [x] What are the circumstances under which promises are better than callbacks?

When callback hell is a very real possiblity. Avoid calling callbacks within callbacks. 

- [x] What does the  `.then()` function do?

Returns a resolution or rejection "Promise" object.

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

An individual Promise behaves as a future value. But there's another way to think of the resolution of a Promise: as a flow-control mechanism -- a temporal this-then-that -- for two or more steps in an asynchronous task. 

##### Promise "Events"
Promise resolution doesn't necessarily need to involve sending along a message, as it did when we were examining Promises as future values. It can just be a flow-control signal. 
#### Thenable Duck Typing
Given that Promises are constructed by the `new Promise(..)` syntax, you might think that `p instanceof Promise` would be an acceptable check. But unfortunately, there are a number of reasons that's not totally sufficient.

Mainly, you can receive a Promise value from another browser window (iframe, etc.), which would have its own Promise different from the one in the current window/frame, and that check would fail to identify the Promise instance. 

Moreover, a library or framework may choose to vend its own Promises and not use the native ES6 `Promise` implementation to do so. In fact, you may very well be using Promises with libraries in older browsers that have no Promise at all. 

It was decided that the way to recognize a Promise (or something that behaves like a Promise) would be to define something called a "thenable" as any object or function which has a `then(..)` method on it. It is assumed that any such value is a Promise-conforming thenable. 

The general term for "type checks" that make assumptions about a value's "type" based on its shape (what properties are present) is called "duck typing" -- "If it looks like a duck, and quacks like a duck, it must be a duck" 

If you try to fulfill a Promise with any object/function value that happens to have a `then(..)` function on it, but you weren't intending it to be treated as a Promise/thenable, you're out of luck, because it will automatically be recognized as thenable and treated with special rules. This is even true if you didn't realize the value has a `then()` on it. 

The standards decision to hijack the previously nonreserved -- and completely general-purpose sounding -- `then()`  property name means that no value (or any of its delegates), either past, present, or future, can have a `then()` function present, either on purpose or by accident, or that value will be confused for a thenable in Promises systems, which will probably create bugs that are really hard to track down. 


#### Promise Trust
##### Calling too early
Somtimes a task finishes synchronously and sometimes asynchronously, which can lead to race conditions.

Promises by definition cannot be susceptible to this concern, because even an immediately fulfilled Promise cannot be observed synchronously.

That is, when you call `then(..)` on a Promise, even if that Promise was already resolved, the callback you provide to `then(..)` will always be called asynchronously

##### Calling too late
A Promise's `then(..)` registered observation callbacks are automatically scheduled when either `resolve(..)` or `reject(..)` are called by the Promise creation capability. Those scheduled callbacks will predictably be fired at the next asynchronous moment. 

It's not possible for synchrounous observation, so it's not possible for a synchronous chain of tasks to run in such a way to in effect "delay" another callback from happening as expected. That is, when a Promise is resolved, all `then(..)` registered callbacks on it will be called, in order, immediately at the next asynchronous opportunity, and nothing that happens inside of one of those callbacks can affect/delay the calling of the other callbacks

##### Promise Scheduling Quirks
It's important to note, there are lots of nuances of scheduling where the relative ordering between callbacks chained off two separate Promises is not reliably predictable. 

If two promises `p1` and `p2` are both already resolved, it should be true that `p1.then(..); p2.then(..)` would end up calling the callback(s) for `p1` before the ones for `p2`. But there are subtle cases where that might not be true. 

To avoid such nuanced nightmares, you should never rely on anything about the ordering/scheduling of callbacks across Promises. In fact, a good practice is not to code in such a way where the ordering of multiple callbacks matters at all. Avoid that if you can. 

##### Never Calling the Callback
Nothing can prevent a Promise from notifying you of its resolution (if it's resolved). If you register both fulfillment and rejection callbacks for a Promise, and the Promise gets resolved, one of the two callbacks will always be called. 

Of course, if your callbacks themselves have JS errors, you may not see the outcome you expect, but the callback will in fact have been called.

##### Calling too few or too many times
By definition, *one* is the appropriate number of times for the callback to be called. The "too few" case would be zero calls, which is the same as the "never" case

The "too many" case is easy to explain. Promises are defined so that they can only be resolved once. If for some reason the Promise creation code tries to call `resolve()` or `reject()` multiple times, or tries to call both, the Promise will accept only the first resolution, and will silently ignore any subsequent attempts

Because a Promise can only be resolved once, any `then()` registered callbacks will only ever be called once (each).

##### Failing to pass along any parameters/environment
Promises can have, at most, one resolution value (fulfillment or rejection)

If you don't explicitly resolve with a value either way, the value is undefined. But whatever the value, it will always be passed to all registered callbacks, either now or in the future.

If you want to pass along multiple values, you must wrap them in another single value that you pass, such as an `array` or an `object`

As for environment, functions in JS always retain their closure of the scope in which they are defined, so they of course would continue to have access to whatever surrounding state you provide. Of course, the same is true of callbacks-only design, so this isn't a specific augmentation of benefit from Promises -- but it's a guarantee we can rely on nonetheless. 

##### Swallowing any errors/exceptions
In the base sense, this is a restatement of the previous point. If you reject a Promise with a reason (error message), that value is passed to the rejection callback(s).

There's something much bigger at play here. If at any point in the creation of a Promise, or in the observation of its resolution, a JS exception error occurs, such as a TypeError or ReferenceError, that exception will be caught will be caught, and it will force the Promise in question to become rejected. 

This is an important detail, because it effectively solves another potential Zalgo moment which is that erros could create a synchronous reaction whereas nonerrors would be asychronous. Promises turn even JS exceptions into asynchronous behavior, thereby reducing the reace condition chances greatly.


##### Trustable Promise?
Promises don't get rid of callbacks at all. They just change where teh callback is passed to. Instead of passing a callback, we get something back and we pass the callback to that something instead. 

One of the most important details of Promises is that they have a solution to this issue as well. Included with the native ES6 `Promise` implementation is `Promise.resolve(..)`

If you pass an immediate, non-Promise, non-thenable value to `Promise.resolve()`, you get a promise that's fulfilled with that value. 

##### Trust Built
Promises are a pattern that augments callbacks with trustable semantics, so that the behavior is more reasonable and more reliable. By uninverting the *inversion of control* of callbacks, we place the control with a trustable system (Promises) that was designed specifically to bring sanity to our async. 

--- 
#### Chain Flow
---
Promises are not just a mechanism for a single-step this-then-that sort of operation. That's the building block, but it turns out we can string multiple Promises together to represent a sequence of async steps.

The key to making this work is built on two behaviors intrinsic to Promises:
- Every time you call `then()` on a Promise, it creates and returns a new Promise, which we can *chain* with.
- Whatever value you return from the `then()` call's fulfillment callback (the first parameter) is automatically set as the fulfillment of the chained Promise (from the first point)
- If the fulfillment or rejection handler returns a Promise, it is unwrapped, so that whatever its resolution is will become the resolution of the chained Promise returned from the current `then()`

While chaining flow control is helpful, it's probably most accurate to think of it as a side benefit of how Promises compose (combine) together, rather than the main intent. Promises normalize asynchrony and encapsulate time-dependent value state, and that is what lets us chain them together in this useful way.

Certainly the sequential expressiveness of the chain (this-then-this-then-this...) is a big improvement over the tangled mess of callbacks. But there's still a fair amount of boilerplate (`then()` and `function(){}`) to wade through. 

##### Resolve, Fulfill, and Reject
Two callbacks are provided. 
- The first is *usually* used to mark the Promise as fulfilled
	- Ultimately, it's just your user code and the identifier names aren't interpreted by the engine to mean anything, so it doesn't *technically* matter. But the words you use can affect not only how you are thinking about the code, but how other developers on your team will think about it. 
	- `Promise.resolve()` creates a Promise that's resolved to the value given to it. 
	- The first callback parameter of the `Promise()` constructor will unwrap either a thenable (identically to `Promise.resolve()`) or a genuine Promise
- The second always marks the Promise as rejected
	- Almost all literature uses `reject()` as its name, and because that's exactly, and only, what it does, that's a very good choice for the name. It's strongly recommended to always use `reject()`

#### Error Handling
--- 
Promises don't use the popular "error-first callback" design style, but instead use "split callbacks" style; there's one callback for fulfillment and one for rejection

You can't get a rejected Promise if you don't use the Promise API validly enough to actually construct a Promise in the first place!

##### Pit of Despair
Promise error handling is unquestionably "pit of despair" design. By default, it assumes that you want any error to be swallowed by the Promise state, and if you forget to observe that state, the error silently languishes/dies in obscurity -- usually despair. 

To avoid losing an error to the silence of a forgotten/discarded Promise, some developers have claimed that a "best practice" for Promise chains is to always end you chain with a final `catch()`

However, you can't just stick another `catch(..)` on the end of that chain, because it too could faill. The last step in any promise chain always has the possibility, even decreasingly so, of dangling with an uncaught error stuck inside an unobserved Promise.

##### Uncaught Handling

Browsers have a unique capability that our code does not have: they can track and know for sure when any object gets thrown away and garbage collected. So, browsers can track Promise objects, and whenever they get garbage collected, they have a rejection in them, the browser knows for sure this was a legitimate "uncaught error", and can thus confidently know it should report it to the developer console.

However, if a Promise doesn't get garbage collected -- it's exceedingly easy for that to accidentally happen through lots of different coding patterns -- the browser's garbage collection sniffing won't help you know and diagnose that you have a silently rejected Promise laying around. 

##### Pit of Success
- Promises could default to reporting (to the developer console) any rejection, on the next Job or event loop tick, if at that exact moment no error handler has been registered for the Promise 
- For the cases where you want a rejected Promise to hold onto its rejected state for an indefinite amount of time before observing, you could call `defer()`, which suppresses automatic error reporting on that promise. 

If a promise is rejected, it defaults to noisily reporting that fact to the developer console. You can opt out of that reporting either implicitly (by registering an error handler before rejection), or explicitly (with `defer()`). In either case, you control the false positives. 

#### Promise Patterns
##### Promise.all([..])
In an async sequence (Promise chain), only one async task is being coordinated at any given moment -- step 2 strictly follows step 1, and step 3 strictly follows step 2. But what about doing two or more steps concurrently (aka "in parallel")?

In classic programming terminology, a "gate" is a mechanism that waits on two or more parallel/concurrent tasks to complete before continuing. It doesn't matter what order they finish it, just that all of them have to complete for the gate to open and let the flow control through.

In the Promise API, we call this pattern `all([..])`.

`Promise.all([..])` expects a single argument, an `array`, consisting generally of Promise instances. The promise returned from the `Promise.all([..])` call will receive a fulfillment message that is an `array` of all the fulfillment messages from the passed in promises, in the same order as specified (regarless of fulfillment order).

The main promise returned from `Promise.all([..])` will only be fulfilled if and when all its constituent promises are fulfilled. If any one of those promises instead is rejected, the main `Promise.all([..])` promise is immediately rejected, discarding all results from any other promises. 

Remember to always attach a rejection/error handler to every promise, even and especially the one that comes back from `Promise.all([..])`. 

##### Promise.race([..])
While  `Promise.all([..])` coordinates multiple Promises concurrently and assumes all are needed for fulfillment, somtimes you only want to respond to the "first Promise to cross the finish line", letting the other Promises fall away.

This patten is classically called a "latch", but in Promises it's called a "race".

`Promise.all([..])` also expects a single `array` argument, containing one or more Promises, thenables, or immediate values. It doesn't make much practical sense to have a race with immediate values, because the first one listed will obviously win -- like a foot race where one runner starts at the finish line. 

Promises cannot be canceled -- and shouldn't be as that would destroy the external immutability trust discuessed in the "Promise Uncancelable" -- so they can only be silently ignored. 

Is there anything in this pattern that proactively frees the reserved resource after the timeout, or otherwise cancels any side effects it may have had? What if all you wanted was to log the fact the promise function timed out?

Some developers have proposed that Promises need a `finally()` callback registration, which is always called when a Promise resolves, and allows you to specify any cleanup that may be necessary. 

#### Promise API Recap 
##### new Promise(..) Constructor
The *revealing constructor* `Promise()` must be used with `new`, and must be provided a function callback that is immediately called. This function is passed two function callbacks that act as resolution capabilities for the promise. We commonly label these `resolve()` and `reject()`

`reject()` simply rejects the promise, but `resolve()` can either fulfill the promise or reject it, depending on what it's passed. If `resolve()` is passed an immediate, non-Promise, non-thenable value, then the promise is fulfilled with that value. 

But if `resolved()` is passed a genuine Promise or thenable value, that value is unwrapped recursively, and whatever its final resolution/state is will be adopted by the promise.

`Promise.resolve()` doesn't do anything if what you pass is already a genuine Promise; it just returns the value directly. So there's no overhed to calling `Promise.resolve()` on values that you don't know the nature of, if one happens to already be a genuine Promise. 

##### then() and catch()
Each Promise instance has `then()` and `catch()` methods, which allow registering of fulfillment and rejection handlers for the Promise. Once the Promise is resolved, one or the other of these handlers will be called, but not both, and it will always be called asynchronously .

`then()` takes one or two parameters, the first for the fulfillment callback, and the second for the rejection callback. If either is omitted or is otherwise passed as a non-function value, a default callback is substituted respectively. The default fulfillment callback simply passes the message along, while the default rejection callback simply rethrows (propagates) the error reason it receives. 

`catch()` takes only the rejection callback as a parameter, and automatically substitutes the default fulfillment callback, as just discussed. 

`then()` and `catch()` also create and return a new promise, which can be used to express Promise chain flow control. If the fulfillment or rejection callbacks have an exception thrown, the returned promise is rejected. If either callback returns an immediate, non-Promise, non-thenable value, that value is set as the fulfillment for the returned promise. If the fulfillment handler specifically returns a promise or thenable value, that value is unwrapped and becomes the resolution of the returned promise. 

##### Promise.all([]) and Promise.race([])
These static helpers on the ES6 `Promise` API both create a Promise as their return value. The resolution of that promise is controlled entirely by the array of promises that you pass in. 

For `Promise.all([])`, all the promises you pass in must fulfill for the returned promise to fulfill. If any promise is rejected, the main returned promise is immediately rejected, too. For fulfillment, you receive an `array` of all the passed in promises' fulfillment values. For rejection, you receive just the first promise rejection reason value. This pattern is classically called a "gate": all must arrive before the gate opens. 

For `Promise.race([])` only the first promise to resolve (fulfillment or rejection) "wins", and whatever that resolution is becomes the resolution of the returned promise. This pattern is classically called a "latch": first one to open the latch gets through. 

If an empty `array` is passed to `Promise.all([])`, it will fulfill immediately, but `Promise.race([])` will hang forever and never resolve.

The ES6 `Promise` API is pretty simply and straightforward. It's at least good enough to serve the most basic of async cases, and is a good place to start when rearranging your code from callback hell to something better. 

#### Promise Limitations 
##### Sequence Error Handling
The limitations of how Promises are designed -- how they chain, specifically -- creates a very easy pitfall where an error in a Promise chain can be silently ignored accidentally. 

Because a Promise chain is nothing more than its constituent Promises wired together, there's no entity to refer to the entire chain as a single thing, which means there's no external way to observe any errors that may occur. 

If you construct a Promise chain that has no error handling in it, any error anywhere in the chain will propagate indefinitely down the chain, until observed. So, in that specific case, having a reference to the last promise in the chain is enough, because you can register a rejection handler there, and it will be notified of any propagated errors. 

##### Single Value
Promises by definition only have a single fulfillment value or a single rejection reason. In simple examples, this isn't that big of a deal, but in more sophisticated scenarios, you may find this limiting. 

The typical advice is to construct a values wrapper (such as an `object` or `array`) to contain these multiple messages. This solution works, but it can be quite awkward and tedious to wrap and unwrap your messages with every single step or your Promise chain. 

##### Single Resolution
One of the most intrinsic behaviors of Promises is that a Promise can only be resolved once (fulfillment or rejection). For many async use cases, you're only retrieving a value once, so this works fine. 

But there's also a lot of async cases that fit into a different model -- one that's more akin to events and/or streams of data. It's not clear on the surface how well Promises can fit into such use cases, if at all. Without significant abstraction on top of Promises, they will completely fall short for handling multiple value resolution. 

##### Inertia
Promises offer a different paradigm, and as such, the approach to the code can be anywhere from just a little different to, in some cases, radically different. You have to be intentional about it, because Promises will not just naturally shake out from the same ol' ways of doing code that have served you thus far. 

The act of wrapping a callback-expecting function to be a Promise-aware function is sometimes referred to as "lifting" or "promisifying". But there doesn't seem to be a standard term for what to call the resultant function other than a "lifted function". 

##### Promise uncancelable
Once you create a Promise and register a fulfillment and/or rejection handler for it, there's nothing external you can do to stop that progression if something else happens to make that task moot. 

DO NOT CANCEL PROMISES

