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


### Asynchronous Chaining

### Multiple Asynchronous Calls with Promise.all()

### Multiple Asynchronous Calls with Promise.race()

### A Promising Future?

## Async/Await

### Promises, Promises
### Asynchronous Awaits in Synchronous Loops
### try/catch Ugliness

## JavaScript Journey
