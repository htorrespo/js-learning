# Flow Control in Modern JS: Callbacks to Promises to Async/Await

JavaScript is regularly claimed to be asynchronous. What does that mean?
How does it affect development? How has the approach changed in recent
years?

Consider the following code:

```javascript
result1 = doSomething1();
result2 = doSomething2(result1);
```

Most languages process each line synchronously. The first line runs and returns
a result. The second line runs once the first has finished regardless of how long it
takes.

## Single-thread Processing

JavaScript runs on a single processing thread. When executing in a browser tab,
everything else stops. This is necessary because changes to the page DOM can’t
occur on parallel threads; it would be dangerous to have one thread redirecting
to a different URL while another attempts to append child nodes.

This is rarely evident to the user, because processing occurs quickly in small
chunks. For example, JavaScript detects a button click, runs a calculation, and
updates the DOM. Once complete, the browser is free to process the next item
on the queue.

Other languages such as PHP also use a single thread but may be
managed by by a multi-threaded server such as Apache. Two requests
to the same PHP page at the same time can initiate two threads
running isolated instances of the PHP runtime.

## Going Asynchronous with Callbacks

Single threads raise a problem. What happens when JavaScript calls a “slow”
process such as an Ajax request in the browser or a database operation on the
server? That operation could take several seconds — even minutes. A browser
would become locked while it waited for a response. On the server, a Node.js
application would not be able to process further user requests.

The solution is asynchronous processing. Rather than wait for completion, a
process is told to call another function when the result is ready. This is known as
a callback, and it’s passed as an argument to any asynchronous function. For
example:

<!--
La solución es el procesamiento asincrónico. En lugar de esperar a que se complete, 
se le dice a un proceso que llame a otra función cuando el resultado esté listo. 
Esto se conoce como devolución de llamada y se pasa como argumento a cualquier 
función asincrónica. Por ejemplo: 
-->

```javascript
doSomethingAsync(callback1);
console.log('finished');

// call when doSomethingAsync completes
function callback1(error) {
  if (!error) console.log('doSomethingAsync complete');
}
```

_doSomethingAsync()_ accepts a callback function as a parameter (only a
reference to that function is passed so there’s little overhead). It doesn’t matter
how long _doSomethingAsync()_ takes; all we know is that _callback1()_ will be
executed at some point in the future. The console will show:

```javascript
finished
doSomethingAsync complete
```

### Callback Hell

Often, a callback is only ever called by one asynchronous function. It’s therefore
possible to use concise, anonymous inline functions:

```javascript
doSomethingAsync(error => {
  if (!error) console.log('doSomethingAsync complete');
});
```
A series of two or more asynchronous calls can be completed in series by
nesting callback functions. For example:

```javascript
async1((err, res) => {
  if (!err) async2(res, (err, res) => {
    if (!err) async3(res, (err, res) => {
      console.log('async1, async2, async3 complete.');
    });
  });
});
```

Unfortunately, this introduces __callback hell__ (http://callbackhell.com/)
— a notorious concept that even has
its own web page! The code is difficult to read, and will become worse when
error-handling logic is added.

Callback hell is relatively rare in client-side coding. It can go two or three levels
deep if you’re making an Ajax call, updating the DOM and waiting for an
animation to complete, but it normally remains manageable.

The situation is different on OS or server processes. A Node.js API call could
receive file uploads, update multiple database tables, write to logs, and make
further API calls before a response can be sent.

## Promises

ES2015 (ES6) introduced Promises (https://www.sitepoint.com/overview-javascript-promises/). 
Callbacks are still used below the surface,
but Promises provide a clearer syntax that chains asynchronous commands so
they run in series (more about that in the next section).

To enable Promise-based execution, asynchronous callback-based functions
must be changed so they immediately return a Promise object. That object
_promises_ to run one of two functions (passed as arguments) at some point in the
future:

- _resolve_: a callback function run when processing successfully completes, and
- _reject_ : an optional callback function run when a failure occurs.

In the example below, a database API provides a _connect()_ method which
accepts a callback function. The outer _asyncDBconnect()_ function immediately
returns a new Promise and runs either _resolve()_ or _reject()_ once a
connection is established or fails:

```javascript
const db = require('database');

// connect to database
function asyncDBconnect(param) {
  return new Promise((resolve, reject) => {
    db.connect(param, (err, connection) => {
      if (err) reject(err);
      else resolve(connection);
    });
  });
}
```

Node.js 8.0+ (https://nodejs.org/api/util.html#util_util_promisify_original)
provides a util.promisify() utility to convert a callback-based
function into a Promise-based alternative. There are a couple of conditions:

- the callback must be passed as the last parameter to an asynchronous
function, and
- the callback function must expect an error followed by a value parameter.

Example:
```javascript
// Node.js: promisify fs.readFile
const
util = require('util'),
fs = require('fs'),
readFileAsync = util.promisify(fs.readFile);
readFileAsync('file.txt');
```
Various client-side libraries also provide promisify options, but you can create
one yourself in a few lines:

```javascript
// promisify a callback function passed as the last parameter
// the callback function must accept (err, data) parameters
function promisify(fn) {
  return function() {
    return new Promise(
      (resolve, reject) => fn(
        ...Array.from(arguments),
        (err, data) => err ? reject(err) : resolve(data)
      )
    );
  }
}

// example
function wait(time, callback) {
  setTimeout(() => { callback(null, 'done'); }, time);
}

const asyncWait = promisify(wait);
ayscWait(1000);
```


### Asynchronous Chaining

Anything that returns a Promise can start a series of asynchronous function calls
defined in _.then()_ methods. Each is passed the result from the previous
_resolve_:

```javascript
asyncDBconnect('http://localhost:1234')
  .then(asyncGetSession) // passed result of asyncDBconnect
  .then(asyncGetUser) // passed result of asyncGetSession
  .then(asyncLogAccess) // passed result of asyncGetUser
  .then(result => { // non-asynchronous function
    console.log('complete'); // (passed result of asyncLogAccess)
    return result; // (result passed to next .then())
  })
  .catch(err => { // called on any reject
    console.log('error', err);
  });
```

Synchronous functions can also be executed in _.then()_ blocks. The returned
value is passed to the next _.then()_ (if any).

The _.catch()_ method defines a function that’s called when any previous
_reject_ is fired. At that point, no further _.then()_ methods will be run. You can
have multiple _.catch()_ methods throughout the chain to capture different
errors.

ES2018 introduces a _.finally()_ method, which runs any final logic regardless
of the outcome — for example, to clean up, close a database connection etc. It’s
currently supported in Chrome and Firefox only, but Technical Committee 39 has
released a .finally() polyfill 
(https://github.com/tc39/proposal-promise-finally/blob/fd934c0b42d59bf8d9446e737ba14d50a9067216/polyfill.js).

```javascript
function doSomething() {
  doSomething1()
  .then(doSomething2)
  .then(doSomething3)
  .catch(err => {
    console.log(err);
  })
  .finally(() => {
    // tidy-up here!
  });
}
```

### Multiple Asynchronous Calls with Promise.all()

Promise _.then()_ methods run asynchronous functions one after the other. If
the order doesn’t matter — for example, initialising unrelated components — it’s
faster to launch all asynchronous functions at the same time and finish when the
last (slowest) function runs _resolve_.

This can be achieved with _Promise.all()_. It accepts an array of functions and
returns another Promise. For example:

```javascript
Promise.all([ async1, async2, async3 ])
  .then(values => { // array of resolved values
    console.log(values); // (in same order as function array)
    return values;
  })
  .catch(err => { // called on any reject
    console.log('error', err);
  });
```

_Promise.all()_ terminates immediately if any one of the asynchronous functions
calls _reject_.


### Multiple Asynchronous Calls with Promise.race()

_Promise.race()_ is similar to _Promise.all()_ , except that it will resolve or reject
as soon as the first Promise resolves or rejects. Only the fastest Promise-based
asynchronous function will ever complete:

```javascript
Promise.race([ async1, async2, async3 ])
  .then(value => { // single value
    console.log(value);
    return value;
  })
  .catch(err => { // called on any reject
    console.log('error', err);
  });
```


### A Promising Future?

Promises reduce callback hell but introduce their own problems.

Tutorials often fail to mention that the whole Promise chain is asynchronous. Any
function using a series of promises should either return its own Promise or run
callback functions in the final _.then()_ , _.catch()_ or _.finally()_ methods.

I also have a confession: Promises confused me for a long time. The syntax often
seems more complicated than callbacks, there’s a lot to get wrong, and
debugging can be problematic. However, it’s essential to learn the basics.

Further Promise resources:

- MDN Promise documentation (https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)
- JavaScript Promises: an Introduction (https://web.dev/promises/)
- JavaScript Promises … In Wicked Detail (https://mattgreer.dev/articles/promises-in-wicked-detail/)
- Promises for asynchronous programming (https://exploringjs.com/es6/ch_promises.html)



## Async/Await

Promises can be daunting, so ES2017 introduced _async_ and _await_ . While it
may only be syntactical sugar, it makes Promises far sweeter, and you can avoid
_.then()_ chains altogether. Consider the Promise-based example below:

pagina 82

```javascript
function connect() {
  return new Promise((resolve, reject) => {
    asyncDBconnect('http://localhost:1234')
    .then(asyncGetSession)
    .then(asyncGetUser)
    .then(asyncLogAccess)
    .then(result => resolve(result))
    .catch(err => reject(err))
  });
}

// run connect (self-executing function)
(() => {
connect();
.then(result => console.log(result))
.catch(err => console.log(err))
})();
To rewrite this using async / await :
```

### Promises, Promises
### Asynchronous Awaits in Synchronous Loops
### try/catch Ugliness

## JavaScript Journey
