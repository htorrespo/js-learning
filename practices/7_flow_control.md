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

### Callback Hell

## Promises

### Asynchronous Chaining

### Multiple Asynchronous Calls with Promise.all()

### Multiple Asynchronous Calls with Promise.race()

### A Promising Future?

## Async/Await

### Promises, Promises
### Asynchronous Awaits in Synchronous Loops
### try/catch Ugliness

## JavaScript Journey
