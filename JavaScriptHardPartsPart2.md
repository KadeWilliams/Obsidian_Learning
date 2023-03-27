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

``` js
/* CHALLENGE 1 */

function sayHowdy() {
  console.log('Howdy');
}

function testMe() {
  setTimeout(sayHowdy, 0);
  console.log('Partnah');
}
// After thinking it through, uncomment the following line to check your guess!
// testMe(); // what order should these log out? Howdy or Partnah first?


/* CHALLENGE 2 */

function delayedGreet() {
  // ADD CODE HERE
  function sayWelcome() {
    console.log("welcome");
  }
  setTimeout(sayWelcome, 3000)
}
// Uncomment the following line to check your work!
// delayedGreet(); // should log (after 3 seconds): welcome


/* CHALLENGE 3 */

function helloGoodbye() {
  function goodbye() {
    console.log('goodbye')
  }
  // ADD CODE HERE
  console.log('hello');
  setTimeout(goodbye, 2000);
}
// Uncomment the following line to check your work!
// helloGoodbye(); // should log: hello // should also log (after 3 seconds): good bye


/* CHALLENGE 4 */

function brokenRecord() {
  // ADD CODE HERE
  function hiAgain() {
    console.log('hi again');
  }
	setInterval(hiAgain, 1000);	
}
// Uncomment the following line to check your work!
// brokenRecord(); // should log (every second): hi again


/* CHALLENGE 5 */

function limitedRepeat() {
  // ADD CODE HERE
  const intervalID = setInterval(hiForNow, 1000);
  
  function hiForNow() {
    console.log('hi for now');
  }
  function clear() {
		clearInterval(intervalID);
  }
	setTimeout(clear, 5000);
}
// Uncomment the following line to check your work!
// limitedRepeat(); // should log (every second, for 5 seconds): hi for now


/* CHALLENGE 6 */

function everyXsecsForYsecs(func, interval, duration) {
  // ADD CODE HERE
  const intID = setInterval(func, interval*1000); 
	function clear() {
    clearInterval(intID); 
  }
  setTimeout(clear, duration*1000); 
}
// Uncomment the following lines to check your work!
// function theEnd() {
//   console.log('This is the end!');
// }
// everyXsecsForYsecs(theEnd, 2, 20); // should invoke theEnd function every 2 seconds, for 20 seconds): This is the end!


/* CHALLENGE 7 */

function delayCounter(target, wait) {
  let val = 0;
  return function() {
    const intID = setInterval(() => {
      val++;
      console.log(val);
      if (val >= target) {
            clearInterval(intID);
      }
    }, wait);
    
  }
}

// UNCOMMENT THESE TO TEST YOUR WORK!
// const countLogger = delayCounter(3, 1000)
// countLogger();
// After 1 second, log 1
// After 2 seconds, log 2
// After 3 seconds, log 3

/* CHALLENGE 8 */

function promised (val) {
  // ADD CODE HERE
	return new Promise((resolve, reject) => {
   setTimeout(() => {
     resolve(val);
   }, 2000); 
  });
}

// UNCOMMENT THESE TO TEST YOUR WORK!
// const createPromise = promised('wait for it...');
// createPromise.then((val) => console.log(val)); 
// will log "wait for it..." to the console after 2 seconds

/* CHALLENGE 9 */

class SecondClock {
  constructor(cb) {
    // ADD CODE HERE
    this.cb = cb;
    this.intID; 
  }
  
  start() {
    this.intID = setInterval(this.cb, 1000); 
  }
  
  reset() {
  	clearInterval(this.intID);   
  }
  
  // ADD METHODS HERE
}

// UNCOMMENT THESE TO TEST YOUR WORK!
// const clock = new SecondClock((val) => { console.log(val) });
// console.log("Started Clock.");
// clock.start();
// setTimeout(() => {
//     clock.reset();
//     console.log("Stopped Clock after 6 seconds.");
// }, 6000);

/* CHALLENGE 10 */

function debounce(callback, interval) {
  // ADD CODE HERE
  let called = false; 
  let counter = 0; 
  return function() {
		let intID;
    if (!called) {
      called = true;
			intID = setInterval(() => counter++, 1)
      return callback();         
    } else if (counter < interval) {
        counter = 0;
        intID = setInterval(() => counter++, 1)
      	return undefined; 
    } else {
				counter = 0;
      	clearInterval(intID);
        intID = setInterval(() => counter++, 1)
        return callback(); 
    }
  }
}

// UNCOMMENT THESE TO TEST YOUR WORK!
function giveHi() { return 'hi'; }
const giveHiSometimes = debounce(giveHi, 3000);
console.log(giveHiSometimes()); // -> 'hi'
setTimeout(function() { console.log(giveHiSometimes()); }, 2000); // -> undefined
setTimeout(function() { console.log(giveHiSometimes()); }, 4000); // -> undefined
setTimeout(function() { console.log(giveHiSometimes()); }, 8000); // -> 'hi'
```




