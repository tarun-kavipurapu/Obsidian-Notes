## Asynchronous JavaScript, AJAX and APIs//IMP

![[Images-attachments/Untitled 27.png|Untitled 27.png]]

![[Images-attachments/Untitled 1 7.png|Untitled 1 7.png]]

![[Images-attachments/Untitled 2 6.png|Untitled 2 6.png]]

![[Images-attachments/Untitled 3 5.png|Untitled 3 5.png]]

![[Images-attachments/Untitled 4 5.png|Untitled 4 5.png]]

XML is not used as communication language anymore betweeen the data servers insead we use JSON which is similar to a javascript object

## Our First AJAX Call: XMLHttpRequest

Find out What is REST?

Old way of sending requests to API using XML

Get starter files from jonas github

when you reload you can observe a very peculiar thing that each time the order in which the countries are appearing changes this is because async calls does not have a specific order the one that is finished first appears first.

But what if we need response in a particular order then we use callbacks in which the next response is only delivered after first one.

```JavaScript
function createCountryElement(country) {
const request = new XMLHttpRequest();
request.open('GET', `https://restcountries.com/v3.1/name/${country}`);
request.send();

request.addEventListener('load', function () {
  console.log(this);
  const [data] = JSON.parse(request.responseText);
  console.log(data);
  const firstshownLanguage = Object.keys(data.languages)[0];
  const currencies = Object.keys(data.currencies)[0];
  const html = `   
        <article class="country">
          <img class="country__img" src="${data.flags.png}" />
          <div class="country__data">
            <h3 class="country__name">${data.name.common}</h3>
            <h4 class="country__region">${data.region}</h4>
            <p class="country__row"><span>👫</span>${(
              +data.population / 1000000
            ).toFixed(1)}</p>
            <p class="country__row"><span>🗣️</span>${
              data.languages[firstshownLanguage]
            }</p>
            <p class="country__row"><span>💰</span>${
              data.currencies[currencies].name
            }</p>
          </div>
        </article>`;
  countriesContainer.insertAdjacentHTML('beforeend', html);
  countriesContainer.style.opacity = 1;
});

}
createCountryElement('usa');
createCountryElement('india');
createCountryElement('pakistan');
```

## How web Works

![[Images-attachments/Untitled 5 5.png|Untitled 5 5.png]]

![[Images-attachments/Untitled 6 5.png|Untitled 6 5.png]]

## Call Back Hell

**Why do we use call backs?**

Callbacks in JavaScript are a way to handle asynchronous operations and ensure that certain code runs only after a particular operation has completed. Here are a few reasons why callbacks are used in JavaScript:

1. Asynchronous Operations: JavaScript often deals with asynchronous operations like fetching data from an API, reading files, or making database queries. Instead of blocking the execution of code until the operation is complete, callbacks allow you to specify what should happen once the operation finishes. This helps to maintain a responsive and non-blocking user interface.
2. Event Handling: JavaScript is frequently used for event-driven programming, where actions like button clicks, mouse movements, or timers trigger specific functions. Callbacks are commonly used to handle these events by passing a function as a callback to be executed when the event occurs.
3. Asynchronous Control Flow: Callbacks enable control flow in asynchronous scenarios. For example, when making multiple API requests in a specific order or handling dependent asynchronous operations, callbacks can be used to execute subsequent steps once the previous step completes.
4. Error Handling: Callbacks also provide a way to handle errors in asynchronous operations. By convention, callbacks accept an error parameter as the first argument, allowing you to handle any errors that occur during the asynchronous operation.

It's worth noting that callbacks have some drawbacks, such as callback hell (nested callbacks that can become hard to read and maintain) and issues with error handling and code organization. To address these drawbacks, newer JavaScript features like Promises and async/await were introduced to provide more concise and readable ways of handling asynchronous operations.

## **Call Back Hell**

Callback hell, also known as the pyramid of doom, refers to a situation in asynchronous JavaScript programming where multiple nested callback functions make the code difficult to read, understand, and maintain. It occurs when there are numerous asynchronous operations that depend on the results of previous operations. Here are some notes about callback hell along with examples:

1. Nested Callbacks: In callback hell, callbacks are nested within each other, leading to deeply indented and hard-to-follow code. Each callback is triggered by the completion of the previous operation.

```Plain
asyncOperation1(function(result1) {
  asyncOperation2(result1, function(result2) {
    asyncOperation3(result2, function(result3) {
      // More nested callbacks...
    });
  });
});
```

1. Error Handling: Error handling becomes complicated as each callback needs its own error handler. This can lead to repetitive and verbose error handling code.

```Plain
asyncOperation1(function(result1, error1) {
  if (error1) {
    // Handle error
  } else {
    asyncOperation2(result1, function(result2, error2) {
      if (error2) {
        // Handle error
      } else {
        asyncOperation3(result2, function(result3, error3) {
          if (error3) {
            // Handle error
          } else {
            // Process result3
          }
        });
      }
    });
  }
});
```

1. Readability and Maintainability: Callback hell makes code difficult to read and understand, especially when there are many asynchronous operations involved. It becomes challenging to trace the flow of execution and reason about the code's behavior.
2. Solution: To alleviate callback hell, several techniques can be used, such as using named functions, modularizing code into separate functions, or adopting control flow libraries like Promises, async/await, or libraries like Async.js or Bluebird.

```Plain
asyncOperation1()
  .then(result1 => asyncOperation2(result1))
  .then(result2 => asyncOperation3(result2))
  .then(result3 => {
    // Process result3
  })
  .catch(error => {
    // Handle errors
  });
```

By using Promises or async/await, the code becomes more linear and easier to understand, avoiding the excessive nesting of callbacks.

Callback hell should be avoided to improve code readability, maintainability, and error handling. Employing modern JavaScript features or libraries can help mitigate the issue and make asynchronous programming more manageable.

## Promises and the Fetch API

![[Images-attachments/Untitled 7 5.png|Untitled 7 5.png]]

![[Images-attachments/Untitled 8 5.png|Untitled 8 5.png]]

![[Images-attachments/Untitled 9 4.png|Untitled 9 4.png]]

  

## Consuming Promises

```JavaScript
function showContent(data){
  const firstshownLanguage = Object.keys(data.languages)[0];
  const currencies = Object.keys(data.currencies)[0];
  const html = `   
        <article class="country">
          <img class="country__img" src="${data.flags.png}" />
          <div class="country__data">
            <h3 class="country__name">${data.name.common}</h3>
            <h4 class="country__region">${data.region}</h4>
            <p class="country__row"><span>👫</span>${(
              +data.population / 1000000
            ).toFixed(1)}</p>
            <p class="country__row"><span>🗣️</span>${
              data.languages[firstshownLanguage]
            }</p>
            <p class="country__row"><span>💰</span>${
              data.currencies[currencies].name
            }</p>
          </div>
        </article>`;
  countriesContainer.insertAdjacentHTML('beforeend', html);
  countriesContainer.style.opacity = 1;

}
function createCountryElement(country) {
const request = fetch(`https://restcountries.com/v3.1/name/${country}`);
// console.log(request);
request.then(response=>response.json())
            .then(data=>showContent(data[0]));
}
createCountryElement('usa');
createCountryElement('india');
```

Remember promises does not get rid of call back but they get rid of call back hell

## Chaining Promises

When using Promises, one common pattern is to chain them together. This involves returning a new Promise from the `then` method of a previous Promise. The idea behind this pattern is to make it easier to perform multiple asynchronous tasks in sequence.

For example, let's say we have a function that fetches data from an API and returns a Promise. We can then chain a `then` method to the Promise that processes the data and returns another Promise. This can be repeated as many times as necessary to perform a series of tasks in sequence.

By chaining Promises, we can avoid the callback hell that can result from nesting multiple callbacks. It also makes it easier to handle errors and propagate them through the chain.

So if you find yourself needing to perform multiple asynchronous tasks in sequence, consider using the chaining pattern with Promises.

  

IF there is a chain we need to keep returning things to continue the chain this ai common mistake the developers make.

```JavaScript
function showContent(data){
  const firstshownLanguage = Object.keys(data.languages)[0];
  const currencies = Object.keys(data.currencies)[0];
  const html = `   
        <article class="country">
          <img class="country__img" src="${data.flags.png}" />
          <div class="country__data">
            <h3 class="country__name">${data.name.common}</h3>
            <h4 class="country__region">${data.region}</h4>
            <p class="country__row"><span>👫</span>${(
              +data.population / 1000000
            ).toFixed(1)}</p>
            <p class="country__row"><span>🗣️</span>${
              data.languages[firstshownLanguage]
            }</p>
            <p class="country__row"><span>💰</span>${
              data.currencies[currencies].name
            }</p>
          </div>
        </article>`;
  countriesContainer.insertAdjacentHTML('beforeend', html);
  countriesContainer.style.opacity = 1;

}


function createCountryElement(country) {
  const request = fetch(`https://restcountries.com/v3.1/name/${country}`);
  // console.log(request);
  request
    .then(response => response.json())
    .then(data => {
      showContent(data[0]);
      console.log();
      const neighbour = data[0].borders[0];

      if (!neighbour) return;

      return fetch(`https://restcountries.com/v3.1/alpha/${neighbour}`);
    })
    .then(response => response.json())
    .then(data => {
      showContent(data[0], 'neighbour');
    });
}
createCountryElement('india');
```

## Rejected Promises

When working with promises, there are two common ways to handle errors when a promise is rejected:

1. Using the `.catch()` method: The `.catch()` method is used to handle any errors that occur during the promise chain. It is chained onto the end of the promise chain and accepts a callback function that will be executed if any promise in the chain is rejected. Here's an example:

```Plain
fetch('<https://api.example.com/data>')
  .then(response => {
    if (!response.ok) {
      throw new Error('Request failed');
    }
    return response.json();
  })
  .then(data => {
    // Process the retrieved data
  })
  .catch(error => {
    // Handle the error
    console.error(error);
  });
```

In the above example, if the `fetch` request encounters an error or if the response status is not in the range of 200-299 (indicating a successful request), the promise will be rejected, and the error will be thrown. The error is then caught by the `.catch()` method, and the specified callback function handles the error.

1. Using a second callback function in the `.then()` method: In addition to the success callback function, the `.then()` method can also accept a second callback function that specifically handles errors. This second callback is equivalent to a `.catch()` method chained immediately after the `.then()` method. Here's an example:

```Plain
fetch('<https://api.example.com/data>')
  .then(response => {
    if (!response.ok) {
      throw new Error('Request failed');
    }
    return response.json();
  }, error => {
    // Handle the error
    console.error(error);
  })
  .then(data => {
    // Process the retrieved data
  });
```

In this approach, the second callback function defined within the `.then()` method handles any errors that occur in the preceding promise. If an error is thrown in the previous `.then()` function or if the promise is rejected, the error callback will be executed.

Both methods allow you to handle errors in rejected promises. The choice between them depends on your coding style and preference. The `.catch()` method is more commonly used and provides a clear separation between the success and error handling logic, while the second callback function approach offers a more concise syntax.

/////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////

`.finally()`-→ The `**.finally()**` method is used in JavaScript promises to specify a callback function that will be executed regardless of whether the promise is fulfilled or rejected. EX(Loading Ba)

  

**The catch method is usually preferrable since it handles the errors globally in the promise chain.**

Applying this on our code

```JavaScript
const renderError = function (msg) {
  countriesContainer.insertAdjacentText('beforeend', msg);
  countriesContainer.style.opacity = 1;
};
function createCountryElement(country) {
  const request = fetch(`https://restcountries.com/v3.1/name/${country}`);
  // console.log(request);
  request
    .then(response => response.json())
    .then(data => {
      showContent(data[0]);
      console.log();
      const neighbour = data[0].borders[0];

      if (!neighbour) return;

      return fetch(`https://restcountries.com/v3.1/alpha/${neighbour}`);
    })
    .then(response => response.json())
    .then(data => {
      showContent(data[0], 'neighbour') 
			.catch(err => {
      console.error(`${err} 💥💥💥`);
      renderError(`Something went wrong 💥💥 ${err.message}. Try again!`);
    })
    .finally(() => {
      countriesContainer.style.opacity = 1;
    });
    });
}
createCountryElement('india');








btn.addEventListener('click', function () {
  getCountryData('portugal');
});
```

## throw new

In JavaScript, the `throw` statement is used to manually throw an exception. When you encounter an error or an exceptional situation in your code, you can use `throw` to create and throw a new `Error` object or a custom error object. This allows you to indicate that something unexpected or erroneous has occurred, and you want to interrupt the normal flow of execution and handle the error gracefully.

The syntax to throw an error is as follows:

```Plain
throw expression;
```

The `expression` can be any value or object, but commonly it is an instance of the `Error` class or its derived classes. You can pass a string message to the `Error` constructor to provide additional information about the error.

Here's an example that throws a new `Error` object:

```Plain
function divide(a, b) {
  if (b === 0) {
    throw new Error('Division by zero is not allowed');
  }
  return a / b;
}

try {
  console.log(divide(10, 0));
} catch (error) {
  console.log('An error occurred:', error.message);
}
```

In the example above, the `divide()` function checks if the divisor `b` is zero. If it is, it throws a new `Error` object with the message "Division by zero is not allowed". The code is wrapped in a `try-catch` block to catch the thrown error. If the error occurs, the `catch` block is executed, and the error message is logged to the console.

By throwing errors, you can communicate exceptional conditions or errors in your code and handle them appropriately with `try-catch` blocks or other error-handling mechanisms. This helps you ensure the reliability and stability of your JavaScript programs.

## Handling Manual Errors That Occur

Make Helper function like this for more clean code

``if(!response.ok)throw new Error(`Country not found: (${response.status})`);``

```JavaScript
const getJSON = function (url,errorMsg='Soomething went wrong') {
return fetch(url).then(response=>{
        if(!response.ok)throw new Error(`Country not found: (${response.status})`);
        return response.json();
        });
};
```

  

## Asynchronous behind the Event Loop//Imp

![[Images-attachments/Untitled 10 4.png|Untitled 10 4.png]]

**If there is only one single Thread of Execution Which means JavaScript can only execute one piece of cod at once than how async code works?**

**Learning how the javascript Concurrency model works behind the scenes**

There will be A series of Slides so Bear with me Understand Clearly

![[Images-attachments/Untitled 11 4.png|Untitled 11 4.png]]

Let us See By executing the code Line By line

![[Images-attachments/Untitled 12 4.png|Untitled 12 4.png]]

**Query Selector IS a ‘Synchronous’ operation so it will take place at call stack.**

![[Images-attachments/Untitled 13 3.png|Untitled 13 3.png]]

**Here the image is loaded asynchronously in javascript and most of the async operations are performed in webAPI.**

![[Images-attachments/Untitled 14 2.png|Untitled 14 2.png]]

Eventhough addEventListner() is created in the execution content the ‘load’ event is an `async` event hence it will take place in WEBAPI

![[Images-attachments/Untitled 15 2.png|Untitled 15 2.png]]

Similary Fetch execution context is created and it happens in WEBAPI.

![[Images-attachments/Untitled 16 2.png|Untitled 16 2.png]]

`then` also happens in the WEBAPI

![[Images-attachments/Untitled 17 2.png|Untitled 17 2.png]]

In the asynchronous JavaScript execution model, tasks are processed in a specific order to ensure proper sequencing. When an asynchronous task, such as a timer, completes its designated time, it is systematically transported to the callback queue. The callback queue holds all the tasks that are ready to be executed.

For example, let's consider a scenario where there are two tasks in the callback queue. The first task takes 1 second to complete, and the second task is a timer set to execute after 5 seconds. In this case, the total time it takes for the timer to be executed is 6 seconds.

Here's a neat summary:

1. The tasks, including timers, are scheduled to execute asynchronously.
2. When a timer reaches its designated time (in this case, 5 seconds), it is placed in the callback queue.
3. The callback queue follows a first-in, first-out (FIFO) order, meaning the tasks are executed in the order they were added.
4. If there is a task ahead of the timer in the callback queue, that task will be processed first.
5. In this scenario, the task before the timer takes 1 second to complete.
6. After the 1-second task is executed, the timer task is then processed.
7. Therefore, it takes a total of 6 seconds for the timer to be executed (1 second for the previous task and 5 seconds for the timer itself).

This orderly execution ensures that tasks are processed in the correct sequence, regardless of the time it takes for each task to complete.

Something Important to Note here is that DOM events also happen in call back queue.

![[Images-attachments/Untitled 18 2.png|Untitled 18 2.png]]

Event Loop takes the elements from CallBack Queue and Executes the in call Stack this is called EventLoop Tick

IMP—>Special Case for Call Backs in `promises`

![[Images-attachments/Untitled 19 2.png|Untitled 19 2.png]]

promises are executed in a special queue called Microtasks Queue

![[Images-attachments/Untitled 20 2.png|Untitled 20 2.png]]

**Again Event Loop Sends them to Stack and MicroTasks Queue have priority over call back Queue**

```JavaScript
setTimeout(() => alert("timeout"));

Promise.resolve()
  .then(() => alert("promise"));

alert("code");
```

What’s going to be the order here?

1. `code` shows first, because it’s a regular synchronous call.
2. `promise` shows second, because `.then` passes through the microtask queue, and runs after the current code.
3. `timeout` shows last, because it’s a macrotask.

## The Event Loop in Practice

![[Images-attachments/Untitled 21 2.png|Untitled 21 2.png]]

![[Images-attachments/Untitled 22 2.png|Untitled 22 2.png]]

Here the Promise is Executed First Because of what we learned Earlier that Promises are called inside MicroTasks Queue and Event Loop only Executes the Call BAck Queue after Microtasks Queue No matter how MUCh TIme it takes.

No matter How much time is taken Microtasks Queue are Executed First

![[Images-attachments/Untitled 23 2.png|Untitled 23 2.png]]

Even For a Heavy Task Promises are Executed first Because of Microtask Queue.

## Building a Simple Promise (without fetch api)

We basically use this to convert call-back behavior to promise behavior this is called promisifying.

```JavaScript
const lotteryPromise = new Promise(function (resolve, reject) {
  console.log('Lotter draw is happening 🔮');
  setTimeout(function () {
    if (Math.random() >= 0.5) {
      resolve('You WIN 💰');
    } else {
      reject(new Error('You lost your money 💩'));
    }
  }, 2000);
});

lotteryPromise.then(res => console.log(res)).catch(err => console.error(err));

// Promisifying setTimeout
const wait = function (seconds) {
  return new Promise(function (resolve) {
    setTimeout(resolve, seconds * 1000);
  });
};

wait(1)
  .then(() => {
    console.log('1 second passed');
    return wait(1);
  })
  .then(() => {
    console.log('2 second passed');
    return wait(1);
  })
  .then(() => {
    console.log('3 second passed');
    return wait(1);
  })
  .then(() => console.log('4 second passed'));
this is same as 
// setTimeout(() => {
//   console.log('1 second passed');
//   setTimeout(() => {
//     console.log('2 seconds passed');
//     setTimeout(() => {
//       console.log('3 second passed');
//       setTimeout(() => {
//         console.log('4 second passed');
//       }, 1000);
//     }, 1000);
//   }, 1000);
// }, 1000);
```

  

## Promisifying the Geolocation API

```JavaScript
const getLocation = function() {
  return new Promise(function(resolve, reject) {
    navigator.geolocation.getCurrentPosition(resolve, reject);
  });
}

getLocation()
  .then(position => {
    console.log(position.coords);
  })
  .catch(error => {
    console.error(error);
  });
```

## async/await function

`**async/await**` is a feature in JavaScript that allows for asynchronous programming using Promises. It provides a more readable and concise syntax for writing asynchronous code compared to traditional callback-based approaches.

The `**async**` keyword is used to define an asynchronous function. It allows the function to use the `**await**` keyword inside its body. An asynchronous function always returns a Promise, which can be resolved with a value or rejected with an error.

The `**await**` keyword is used to pause the execution of an asynchronous function until a Promise is resolved. It can only be used within an `**async**` function. When `**await**` is used on a Promise, the function is suspended and resumes execution only when the Promise is resolved. The value of the resolved Promise is then returned.

Here .then()chaining is replaced by `await` and simply assigned to a variable and it is further awaited using `await` keyword.

```JavaScript
const getPosition = function () {
  return new Promise(function (resolve, reject) {
    navigator.geolocation.getCurrentPosition(resolve, reject);
  });
};

const whereAmI = async function (country) {
  const pos = await getPosition();
  const { latitude: lat, longitude: lng } = pos.coords;

  const res = await fetch(`https://geocode.xyz/${lat},${lng}?geoit=json`);

  // fetch(`https://restcountries.eu/rest/v2/name/${country}`).then(res => console.log(res));
  const res = await fetch(`https://restcountries.eu/rest/v2/name/${country}`);
  const data = await res.json();
  console.log(data);
  renderCountry(data);
};

whereAmI('portugal');
console.log('FIRST');
```

## try/error handling

Certainly! The `try` and `catch` keywords are used in JavaScript for error handling within a `try` block. The `try` block contains the code that might throw an error, and the `catch` block is used to handle any potential errors that occur within the `try` block.

Here's an example that demonstrates the usage of `try` and `catch`:

```Plain
function divide(a, b) {
  try {
    if (b === 0) {
      throw new Error('Divide by zero error');
    }
    return a / b;
  } catch (error) {
    console.log('An error occurred:', error.message);
  }
}

console.log(divide(10, 2)); // Output: 5
console.log(divide(10, 0)); // Output: An error occurred: Divide by zero error
```

In the above example, the `divide` function takes two arguments `a` and `b` and attempts to perform the division `a / b`. Inside the `try` block, it checks if `b` is zero and throws an `Error` with a custom message if it is. If no error occurs, the function returns the result of the division.

If an error is thrown within the `try` block, the code execution jumps to the corresponding `catch` block. In this case, the `catch` block logs an error message to the console using `error.message`. The program continues executing after the `catch` block.

In the example, `divide(10, 2)` returns the result of the division, which is `5`, and it is logged to the console. However, `divide(10, 0)` throws an error within the `try` block because dividing by zero is not allowed. The error is caught in the `catch` block, and the error message is logged to the console.

Using `try` and `catch` allows you to handle errors gracefully and take appropriate actions when an error occurs during the execution of critical code sections.

  

Showing using countries and neighbouring countries using `async await` and `try`and `catch`

```JavaScript
async function createCountryElement(country) {
  try {
    const response = await fetch(`https://restcountries.com/v3.1/name/${country}`);
    const data = await response.json();
    showContent(data[0]);

    const neighbour = data[0].borders[0];

    if (!neighbour) return;

    const neighbourResponse = await fetch(`https://restcountries.com/v3.1/alpha/${neighbour}`);
    const neighbourData = await neighbourResponse.json();
    showContent(neighbourData[0], 'neighbour');
  } catch (err) {
    console.error(`${err} 💥💥💥`);
    renderError(`Something went wrong 💥💥 ${err.message}. Try again!`);
  } finally {
    countriesContainer.style.opacity = 1;
  }
}

createCountryElement('india');
```

## Returning values from async function

  

`async` function will always return a promise even if we return something else using `return` keyword it will become the fullfilled value of that `promise`

![[Images-attachments/Untitled 24 2.png|Untitled 24 2.png]]

Here the function `whereAmI()` is returing the string `You are in city` but the output of console log is

![[Images-attachments/Untitled 25 2.png|Untitled 25 2.png]]

it is a promise as expected to get the string we need to use then() method to receive the promise and console.log() it

![[Images-attachments/Untitled 26 2.png|Untitled 26 2.png]]

![[Images-attachments/Untitled 27 2.png|Untitled 27 2.png]]

also you can observe here that a async function is always executed after normal functionss due to microtasks queue as we discussed earlier.

![[Untitled 28.png]]

here we are combining the philosophy of both async and promises so it is good to have only the async/await function in here using IIFE(immediately invoked functions).

![[Untitled 29.png]]

here we are using only `async`

R

## Running promises in parallel

_**When ever you have multiple promises to run in parallel and one does not depend on other.this is a very good strategy to use.**_

To run `async/await` Promises in parallel, you can utilize `Promise.all()` or `Promise.allSettled()`. These methods enable you to execute multiple Promises concurrently and await their completion as a group. Here's an example:

```JavaScript
async function fetchData(url) {
  // Simulating an asynchronous operation
  return new Promise(resolve => {
    setTimeout(() => {
      resolve(`Data from ${url}`);
    }, Math.random() * 2000);
  });
}

async function fetchAllData() {
  const urls = ['<https://example.com/api1>', '<https://example.com/api2>', '<https://example.com/api3>'];

  try {
    const promises = urls.map(url => fetchData(url)); // Create an array of Promises
    const results = await Promise.all(promises); // Wait for all Promises to resolve

    console.log(results); // Output: Array of fetched data
  } catch (error) {
    console.log('An error occurred:', error);
  }
}

fetchAllData();
```

In the above example, the `fetchAllData` function demonstrates parallel execution of multiple asynchronous operations using `Promise.all()`. It creates an array of Promises by calling the `fetchData` function for each URL. Each `fetchData` Promise simulates an asynchronous operation with a random delay using `setTimeout`.

By passing the array of Promises to `Promise.all()`, we await the completion of all Promises concurrently. If all Promises are resolved, an array of their results is returned. Otherwise, if any Promise is rejected, the `catch` block handles the error.

In this case, when all Promises are resolved, the results are logged to the console. Each element in the `results` array corresponds to the resolved value of each Promise, representing the data fetched from different URLs.

You can adapt this example to fit your specific use case by modifying the `fetchData` function and the URLs array according to your needs.

  

## Other Promise Combinators: race, allSettled and any

`Promise.race()` is a method in JavaScript that allows you to race multiple Promises against each other and return the result of the first Promise that resolves or rejects. It can be useful when you want to perform multiple asynchronous operations and only need the result of the fastest one.

Here's an example to demonstrate the usage of `Promise.race()`:

```Plain
function delay(ms) {
  return new Promise(resolve => setTimeout(resolve, ms));
}

async function racePromises() {
  const promise1 = delay(2000).then(() => 'Promise 1');
  const promise2 = delay(1500).then(() => 'Promise 2');
  const promise3 = delay(1000).then(() => 'Promise 3');

  try {
    const result = await Promise.race([promise1, promise2, promise3]);
    console.log('First resolved promise:', result);
  } catch (error) {
    console.log('An error occurred:', error);
  }
}

racePromises();
```

In the above example, the `racePromises` function creates three Promises using the `delay` function with different durations. Each Promise resolves with a different string indicating its identity.

By passing an array of Promises `[promise1, promise2, promise3]` to `Promise.race()`, the function waits for the first Promise to resolve or reject. The result of the fastest Promise is returned, and if any Promise is rejected, the `catch` block handles the error.

In this case, the Promises have different delays, and the Promise that resolves first will determine the result. The result is then logged to the console using `console.log()`.

Please note that if the fastest Promise resolves successfully, the other Promises will continue to execute in the background, but their results will not be used. `Promise.race()` is primarily focused on capturing the fastest Promise's result or error.

You can customize the example by adjusting the delays or adding more Promises to experiment with different scenarios.

![[Untitled 30.png]]

here we get only one array result but we get whichever is the fastest it may change every time.

getJSON is an user defined funnction in the course in GitHub

  

`Promise.allSettled()` is a method in JavaScript that takes an array of Promises and returns a Promise that is fulfilled with an array of objects representing the settlement of each Promise, regardless of whether they were resolved or rejected.

Each object in the resulting array contains the following properties:

- `status`: Represents the status of the Promise. It can have one of the following values: `'fulfilled'` if the Promise was resolved successfully, or `'rejected'` if the Promise was rejected.
- `value` (optional): The value of the resolved Promise if `status` is `'fulfilled'`.
- `reason` (optional): The reason or error object of the rejected Promise if `status` is `'rejected'`.

Here's an example to illustrate the usage of `Promise.allSettled()`:

```Plain
function delay(ms) {
  return new Promise(resolve => setTimeout(resolve, ms));
}

async function fetchUser(userId) {
  // Simulating an asynchronous operation to fetch a user
  await delay(1000);

  if (userId === 1) {
    return { id: 1, name: 'John' };
  } else {
    throw new Error('User not found');
  }
}

async function fetchAllUsers() {
  const userIds = [1, 2, 3];

  const promises = userIds.map(userId => fetchUser(userId));
  const results = await Promise.allSettled(promises);

  results.forEach((result, index) => {
    if (result.status === 'fulfilled') {
      console.log(`User ${userIds[index]}:`, result.value);
    } else {
      console.log(`User ${userIds[index]}:`, result.reason.message);
    }
  });
}

fetchAllUsers();
```

In the above example, the `fetchAllUsers` function demonstrates the usage of `Promise.allSettled()` to fetch multiple users concurrently. It creates an array of Promises by calling the `fetchUser` function for each user ID. The `fetchUser` function simulates an asynchronous operation with a delay of 1 second using `delay`, and depending on the user ID, it either resolves with the user object or throws an error.

By passing the array of Promises to `Promise.allSettled()`, we await the completion of all Promises and receive an array of settlement objects in the `results` array. We then iterate over the `results` array and log the information based on the `status` property. If a Promise was fulfilled, we log the user object. If a Promise was rejected, we log the error message.

In this case, the example fetches three users with user IDs 1, 2, and 3. Since the `fetchUser` function intentionally throws an error for user ID 2, the corresponding Promise will be rejected. The settlement status and relevant information are logged for each user.

You can modify the example by adjusting the `fetchUser` function or adding more user IDs to observe the behavior of `Promise.allSettled()` with different scenarios.