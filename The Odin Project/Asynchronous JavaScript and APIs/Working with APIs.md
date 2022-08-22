# Working with APIs
## Introduction
One of the most powerful things a web dev can do is fetching data from a server and displaying it creatively on their site. In many cases, the server solely exists for that specific site. The server could contain blog posts, user data, high scores for a game or anything else. In other cases, the server is an open service that serves data to anyone that wants to use it. In either case, the methods of accessing and then using that data are essentially the same. 

## Learning Outcomes
- [ ] What is an API
- [ ] How access to an API works
- [ ] How to fetch and extract data from an API
- [ ] Why your API request might be blocked by the browser, and how to fix this

## APIs
Servers that are created for serving data for external use are often referred to as APIs or Application Programming Interfaces

There are multiple ways of requesting data from an API, but all of them basically do the same thing. For the most part, APIs are accessed through URLs, and the specifics of how to query these URLs change based on the specific service you are using. 

In most cases you will have to create an account and request an "API key" from the API service before attempting to fetch data from their endpoints. Once obtained, an API key will usually have to be included with every data request, such as another URL query string parameter. 

On one hand, issuing API keys allows an API service to better track abuse of their systems and data. On the other hand, it can also be a way for those services to mitigate and recuperate operating costs. While a single request to an API might cost a fraction of a penny, imagine using that API to create an amazing weather app that gets used all over the world... you could easily have thousands of people accessing that data every minute! The cost to handle that traffic could quickly balloon up to significatn sums for the API service. 

Because your API key is your key to these services and data, securing them is an important habit, especially if you are using a paid tier. There are plenty of bots that crawl GitHub repositories solely for hardcoded/unsecured API keys allowing bad agents to then access and utilize the services and data you've paid for. 

## Fetching Data

Meet `fetch()` 
```js
fetch("url.com/url1/something")
.then(function(response) {

})
.catch(function(err) {

})
```

## Using the Fetch API
The Fetch API provides a JavaScript interface for accessing and manipulating parts of the HTTP pipeline, such as requests and responses. It also provides a global `fetch()` method that provides an easy, logical way to fetch resources asynchronously across the network. 

Fetch provides a single logical place o define other HTTP-related concepts such as CORS and extensions to HTTP.

The `fetch` specification differs from `jQuery.ajax()` in the following specific ways:

- The Promise returned from `fetch()` won't reject on HTTP error status even in the response is an HTTP 404 or 500. Instead, as soon as the server responds with headers, the Promise will resolve normally (with the ok property of the response set to false if the response isn't in the range 200-299), and it will only reject on network failure or if anything prevented the request from completing.
- Unless `fetch()` is called with the credentials option set to `include`, `fetch()`: 
	- won't send cookies in cross-origin requests
	- won't set anycookies sent back in cross-origin responses
	- the default credentials policy changed to same-origin. 

A basic fetch request is really simple to set up. 

```js
fetch('http://example.com/movies.json')
.then((response) => response.json())
.then((data) => console.log(data));
```

Here we are fetching a JSON file across the network and printing it to the console. The simplest use of `fetch()` takes one argument -- the path to resource you want to fetch -- and does not directly return the JSON response body bu instead returns a promise that resolves with a Response object. 

The Response object, in turn, does not directly contain the actual JSON response body but is instead a representation of the entire HTTP response. So, to extract the JSON body content from the Response object, we use the json() method, which returns a second promise that resolves with the result of parsing the response body text as JSON.

### Supplying the request options
The `fetch()` method can optionally accetp a second parameter, an `init` object that allows you to control a number of different settings:
- method
- mode
- cache
- credentials
- headers
- redirect
- referrerPolicy
- body

### Sending a request with credentials included
To cause browsers to send a request with credentials included on both same-origin and cross-origin calls, add `credentials: 'include'` to the `init` object you pass to the `fetch()` method.
```js
fetch('https://example.com', {
	credentials: 'include'
})
```

### Uploading JSON data
Use `fetch()` to POST JSON-encoded data

```js
JSON.stringify(data)
```

### Headers
The Headers interface allows you to create your own headers object via the Headers() constructor. A headers object is a simple multi-map of names to values:

```js 
const myHeaders = new Headers();
myHeaders.append('Content-Type', 'text/plain');
// OR 
const myHeaders = new Headers({
	'Content-Type': 'text/plain'
})
```

### Response Objects
The most common response properties you'll use are:
- Response.status - An Integer (default value 200) containing the repsonse status code
- Response.statusText - A string (default value ""), which corresponds to the HTTP status code message. 
- Response.ok - Shorthand for checking that the status is in the range 200-299 inclusive. This returns a boolean value. 

