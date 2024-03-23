## Node V8 Libuv and C

![[Images-attachments/Untitled 34.png|Untitled 34.png]]

## Processes Threads and the Thread Pool

- Node.js runs in a single thread, which can be easily blocked, regardless of the number of users accessing the application.
- The event loop is where most of the work in a Node.js application is done.
- Some heavy tasks, such as file operations, cryptography, compression, and DNS lookups, are offloaded to the thread pool to prevent blocking the main thread.
- The thread pool consists of four additional threads that are separate from the main thread, and it can be configured up to 128 threads.
- The event loop automatically offloads expensive tasks to the thread pool behind the scenes, ensuring they don't block the event loop.
- all the process outside of the event loop are executed firstt=.

![[Images-attachments/Untitled 1 11.png|Untitled 1 11.png]]

## The Node js Event Loop

Sure! Here are the important points from the transcript that you can include in your notes:

- Node.js runs in a single thread, which makes it susceptible to blocking.
- The event loop is the heart of the Node.js architecture and is responsible for executing code inside callback functions.
- Node.js uses an event-driven architecture, where events are emitted when tasks are completed, and the event loop calls the associated callback functions.
- The event loop has multiple phases, each with its own callback queue: timer callbacks, I/O polling and execution of I/O callbacks, setImmediate callbacks, and close callbacks.
- After each phase, the event loop checks if there are any pending tasks in the nextTick queue or microtasks queue (for resolved promises) and executes them immediately.
- The event loop continues running as long as there are timers or I/O tasks pending in the background.
- Blocking the event loop can lead to performance issues, so it's important to avoid synchronous operations, complex calculations, large JSON objects, and complex regular expressions in the event loop.
- It's essential to understand the event loop to write performant code and debug issues effectively in Node.js.

Event loop is the Heart of the Node js Architecture

![[Images-attachments/Untitled 2 10.png|Untitled 2 10.png]]

![[Images-attachments/Untitled 3 9.png|Untitled 3 9.png]]

![[Images-attachments/Untitled 4 7.png|Untitled 4 7.png]]

## Demonstrating in Practical Scenario

Here are the important points from the transcript:

1. The event loop is responsible for executing asynchronous JavaScript code.
2. Simulating the event loop can be challenging, but experiments can be conducted to understand its behavior.
3. Code executed outside of callback functions runs immediately in the top-level code.
4. Callback functions are executed within the event loop.
5. The event loop waits for callbacks to be executed during the polling phase.
6. The order of execution in the event loop can be influenced by different types of timers and callback functions.
7. SetTimeout and SetImmediate timers have different execution orders in the event loop.
8. Process.nextTick callbacks are executed immediately after the current phase of the event loop.
9. The naming of SetImmediate and Process.nextTick can be misleading and confusing.
10. The thread pool is used for offloading heavy operations from the event loop.
11. The default size of the thread pool is four, but it can be modified using environmental variables.
12. Synchronous functions, such as readFileSync, block the event loop until they complete.
13. Asynchronous functions, such as readFile, allow the event loop to continue processing other tasks.
14. Blocking code execution can cause delays in the event loop.

These points cover the main concepts discussed in the transcript regarding the event loop, timers, callback functions, thread pool, and blocking code.

Without functions being in event loop

```JavaScript
const fs  = require('fs');

setTimeout(()=>console.log('Timer 1 is finished'),0);
setImmediate(()=>console.log('Immedite 1 finished'));

fs.readFile('test-file.txt',()=>{
    console.log('I/o finished')
});
console.log('Hello from the top-level code');
//OUTPUT:

//Hello from the top-level code
//Timer 1 is finished
//Immedite 1 finished
//I/o finished
```

Functions in Event loop

```JavaScript
const fs  = require('fs');

    setTimeout(() => console.log("Timer 2 is finished"), 0);
    setImmediate(() => console.log("Immedite 1 finished"));
fs.readFile('test-file.txt',()=>{
        console.log('------------------------------------------------------------------>');
    setTimeout(() => console.log("Timer 1 is finished"), 0);
    setTimeout(() => console.log("Timer 2 is finished"), 3000);
    setImmediate(() => console.log("Immedite 1 finished"));
    
    process.nextTick(()=>console.log("Procecess Tick"));
    Promise.resolve(console.log("Promise here")).then();
    console.log('I/o finished')

});
console.log('Hello from the top-level code');
console.log('Hello from the top-level code');

//OUTPUT:->
//Hello from the top-level code
//Timer 2 is finished
//Immedite 1 finished
------------------------------------------------------------------>
//Promise here
//I/o finished
//Procecess Tick
//Immedite 1 finished
//Timer 1 is finished
//Timer 2 is finished
```

Sizze of the thread pool can be changed and execution time of the thhread ppool can also be observed see this in the video. **The Event Loop in Practice jonas**

## Events and Event Driven Architecture

- Node.js architecture heavily relies on events and event-driven architecture.
- Node's core modules, such as HTTP, file system, and timers, are built around an event-driven architecture.
- Event emitters are objects in Node that emit named events when something important happens, such as a request hitting a server or a file finishing to read.
- Event listeners are set up by developers to pick up these emitted events and execute callback functions attached to each listener.
- Node's HTTP module utilizes the event-driven architecture to handle server requests. When a request hits the server, an event called "request" is emitted, and the attached listener's callback function is automatically called to send data back to the client.
- The event emitter logic used in Node.js is known as the observer pattern in JavaScript programming.
- The observer pattern allows for decoupling of modules, as they emit events that other functions, even from different modules, can respond to.
- Using an event-driven architecture makes it easier to react multiple times to the same event by setting up multiple listeners.
- The event-driven architecture provides a more straightforward way to handle and react to events, promoting modularity and decoupling of code.

Note: The transcript seems to be cut off abruptly at the end.

![[Images-attachments/Untitled 5 7.png|Untitled 5 7.png]]

## Events in Practice

Sure! Here are the important points to note:

- To use the built-in Node.js events, you need to require the `**events**` module and import the `**EventEmitter**` class.
- Create an instance of the `**EventEmitter**` class to create a new emitter: `**const myEmitter = new EventEmitter();**`
- Event emitters can emit named events using the `**emit()**` method, such as `**myEmitter.emit('eventName', eventData);**`.
- Set up event listeners using the `**on()**` method, which takes the event name and a callback function as arguments, like `**myEmitter.on('eventName', callbackFunction);**`.
- Event listeners are executed when the corresponding event is emitted.
- Multiple listeners can be set up for the same event, and they will be executed synchronously in the order they were defined.
- Event listeners can receive additional data passed as arguments in the `**emit()**` method, and they can access it in the callback function.
- It is a best practice to create a new class that extends the `**EventEmitter**` class when using events in real-life applications.
- Many built-in Node.js modules, such as `**http**` or `**fs**`, implement events internally by inheriting from the `**EventEmitter**` class.
- The event-driven nature of Node.js allows you to listen to events emitted by modules and react accordingly.

```JavaScript
const EventEmmiter = require('events');
const http = require('http');


class Sales extends EventEmmiter{
    constructor(){
        super();
    }
}
const myEmitter = new Sales() ;

myEmitter.on('NewSale',()=>{
    console.log('Triggered')
});
myEmitter.on('NewSale',()=>{
    console.log('Triggered again')
});
myEmitter.on('NewSale',stock =>{
    console.log(`Stock quantity ${stock}`)
});
myEmitter.emit('NewSale',9);

//OUTPUT
Triggered
Triggered again
Stock quantity 9
```

```JavaScript
const http = require('http')
const server = http.createServer();
//internally http inherits its methods from EventEmitter.
server.on('request',(req,res)=>{
console.log('request made');
res.end('Request made');
})
server.on('request',(req,res)=>{//remember .on is alwayys used to handle events 
console.log('request made again');
})
server.on('close',()=>{
    console.log('Server Cllosed');
})
server.listen(8000,'127.0.0.1',()=>{
    console.log('Waiting for requests...')
})
```

## Introduction to Streams

Sure! Here are some important points to note about streams in Node.js:

1. Streams are a way to handle continuous data flows in Node.js efficiently. They allow processing of data piece by piece, making it more memory-efficient and enabling processing to start before all the data arrives.
2. There are four fundamental types of streams in Node.js: readable streams, writable streams, duplex streams, and transform streams. Readable and writable streams are the most important ones.
3. Readable streams allow data consumption and are used to read data from sources like HTTP requests or files. They emit events such as "data" (when new data is available) and "end" (when there is no more data).
4. Streams in Node.js are instances of the EventEmitter class, which means they can emit and listen to named events.
5. Important functions for readable streams are `pipe` (to pass data from one stream to another) and `read` (to read data from the stream).
6. Writable streams are used for writing data, such as sending an HTTP response. They emit events like "drain" (when the write buffer becomes empty) and "finish" (when all data has been written).
7. Important functions for writable streams are `write` (to write data into the stream) and `end` (to indicate the end of writing).
8. Duplex streams are both readable and writable at the same time, allowing bidirectional data flow. WebSockets are an example of duplex streams.
9. Transform streams are a type of duplex stream that can modify or transform the data as it is being read or written. An example is the `zlib` core module, used for compressing data.
10. The events and functions mentioned here are for consuming pre-implemented streams in Node.js. Implementing custom streams is a separate topic and not covered in this context.

Remember, understanding how to consume streams is more important for most applications rather than implementing them from scratch.

These are the key points to note about streams in Node.js.

![[Images-attachments/Untitled 6 6.png|Untitled 6 6.png]]

![[Images-attachments/Untitled 7 6.png|Untitled 7 6.png]]

## Streams in Practice

Notes from "037 Streams in Practice":

- The transcript discusses working with streams in Node.js.
- The goal is to read a large text file from the file system and send it to the client.
- Multiple solutions are explored, starting with the most basic and moving towards the best solution.
- Solution 1: Read the file into a variable and then send it to the client using the response object. However, this approach is not suitable for large files or high traffic as it loads the entire file into memory.
- Solution 2: Use streams to avoid loading the entire file into memory. Create a readable stream from the file and send each chunk of data to the client as a response. This approach allows for streaming the file content to the client, avoiding memory issues.
- Solution 3: Improve Solution 2 by using the pipe operator, which allows piping the output of a readable stream directly into the input of a writable stream. This solves the problem of back pressure, where the response stream cannot handle the incoming data as fast as it is received from the file.
- The pipe operator is an elegant and straightforward solution for consuming and writing streams, providing a more efficient way to handle data flow.
- The transcript also covers error handling, logging errors to the console, and setting appropriate status codes for error responses.
- The speaker highlights the importance of using the correct syntax for methods and events, as different frameworks may have variations.
- The lecture concludes by testing the implemented solutions and verifying their functionality.

```JavaScript
const fs = require("fs");
const server = require("http").createServer();

server.on("request", (req, res) => {
  // Solution 1
  // fs.readFile("test-file.txt", (err, data) => {
  //   if (err) console.log(err);
  //   res.end(data);
  // });

  // Solution 2: Streams
  // const readable = fs.createReadStream("test-file.txt");
  // readable.on("data", chunk => {
  //   res.write(chunk);
  // });
  // readable.on("end", () => {
  //   res.end();
  // });
  // readable.on("error", err => {
  //   console.log(err);
  //   res.statusCode = 500;
  //   res.end("File not found!");
  // });

  // Solution 3
  const readable = fs.createReadStream("test-file.txt");
  readable.pipe(res);
  // readableSource.pipe(writeableDest)
});

server.listen(8000, "127.0.0.1", () => {
  console.log("Listening...");
});
```

  

## How Requiring Modules Really work

![[Images-attachments/Untitled 8 6.png|Untitled 8 6.png]]

![[Images-attachments/Untitled 9 5.png|Untitled 9 5.png]]

![[Images-attachments/Untitled 10 5.png|Untitled 10 5.png]]

![[Images-attachments/Untitled 11 5.png|Untitled 11 5.png]]

## Modules

Title: "039 Requiring Modules in Practice"

- Node.js wraps code in modules into a wrapper function.
- To demonstrate this, create a new file named "modules.js" and log the arguments array, which contains the values passed into a function.
- The wrapper function has five arguments:
    1. "export" (currently empty, used for exporting values)
    2. "require" function (used for importing other modules)
    3. "module" object (represents the current module)
    4. "filename" (name of the current module)
    5. "dirname" (directory name of the current module)
- The code inside a module is wrapped within the wrapper function, as proven by the existence of these arguments.
- By requiring the "module" module, we can access the wrapper function, which contains the template used by Node.js.
- Exporting and importing data from one module to another:
    - Create a new module, "test module 1", and define a calculator class with methods for addition, multiplication, and division.
    - Use `**module.exports**` to export the calculator class.
    - Import the module using `**require**`, and assign it to a variable (e.g., `**calculator1**`).
    - Use the methods of the imported class for calculations (e.g., `**calculator1.add(2, 5)**`).
- An alternative way to export values is by using the shorthand "exports" object.
    - Assign properties to the "exports" object (e.g., `**exports.add = (a, b) => a + b**`).
    - When importing the module, the "exports" object becomes accessible.
    - Destructuring can be used to selectively import specific properties from the "exports" object.
- Node.js implements module caching.
- When a module is required multiple times, it is loaded only once, and subsequent calls retrieve the cached module.
- The caching mechanism prevents the execution of top-level code multiple times but allows the functions within the module to be called repeatedly.

Note: The lecture covers the concepts of module wrapping, exporting/importing data using `**module.exports**`, exporting using the shorthand "exports" object, and module caching in Node.js.