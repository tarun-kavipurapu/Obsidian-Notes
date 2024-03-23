![[Untitled.png]]

![[Untitled 1.png]]

## Lecture 89: A High-Level overview of JS

From  
<  
[https://rgbk21.github.io/Code/JavaScript/JavaScript.html#Module7_Udemy](https://rgbk21.github.io/Code/JavaScript/JavaScript.html#Module7_Udemy)>

- JS is:
- High Level  
    Language: In languages like C, you must manage your memory. High-level  
    languages abstract away the memory management (apart from doing other things).  
    
- Garbage Collected: Automatic garbage  
    collection is one of the tools that take memory management away from developers.  
    
- Interpreted or Just-In-Time (JIT) Compiled:
- Multi-Paradigm: Paradigm refers to a style of writing code. This can be  
    imperative or declarative. JS can support multiple styles of writing code, such  
    as, Procedural (organizing code in a very linear way so that the code runs  
    top-to-bottom with functions in between), Object-Oriented, Functional  
    
- Prototype-based Object-Oriented:
- First-Class functions: functions are treated as regular variables, i.e., we can  
    pass them into other functions, and return them from functions. This is what  
    enables functional programming in Js.  
    
    ![[Untitled 2.png]]
    
- Dynamically typed: In JS we do not assign datatypes to variables. The datatypes  
    are known only once the JS engine executes our code. Also, the type of  
    variables can easily be changed by reassigning variables.  
    

- Single-Threaded: JS runs in one single thread.
- Non-Blocking Event-Loop: Long-running tasks are executed by the event loop in  
    the "background" and then put back in the main thread once their  
    execution is complete.  
    

**Lecture 90: JS Engine and RunTime**

- JS Engine  
    is, simply put, the code that executes the JS code. Every browser has it's own  
    JS engine. The most well-known JS engine is Google's V8. The V8 engine powers  
    Google Chrome, but also nodeJS. Other browsers have their own JS engines.  
    
- A JS engine contains a call stack and a heap. The Call Stack is where our JS code is  
    executed using something known as the Execution Context. The heap is an  
    unstructured memory pool that stores all the objects that our application  
    needs.  
    

![[Untitled 3.png]]

- **What is Difference between Compiled languages and Interpreted languages:**
    
      
    
    [https://stackoverflow.com/questions/3265357/compiled-vs-interpreted-languages](https://stackoverflow.com/questions/3265357/compiled-vs-interpreted-languages)
    
    Before the code can be run by the target system (think your laptop CPU), it needs to be converted to machine code (sequence of 0s and 1s). So your JS code has to be converted to machine code. Now how is this going to happen? Compilation is the process by which the entire source code (your code) is converted into machine code at once, and written to a binary file that can be executed on any computer. Any application that you run your machine has been already previously compiled into a binary executable.
    
    ![[Untitled 4.png]]
    
    An interpreted language, on the other hand, consists of an interpreter that runs through the source code and executes it line-by-line. So, basically, the code is read and executed at the same time. JS used to be an interpreted language, but modern JS isn't. This is primarily because interpreted languages are slow, and JS couldn't keep up with the development of the browser.
    
    [![](https://rgbk21.github.io/Code/JavaScript/imgs/UdemyCourse/Module8/Image4.png)](https://rgbk21.github.io/Code/JavaScript/imgs/UdemyCourse/Module8/Image4.png)
    
    Modern JS uses a combination of Compilation and Interpretation, which is known as Just-In-Time (JIT) compilation.
    
    ![[Untitled 5.png]]
    
    - TODO: I still don't understand what is actually happening with the JIT Compilation.
    - The first step is parsing the code that reads the JS code and converts it into a datastructure known as the AST (Abstract Syntax Tree). The compilation process takes the generated AST and converts it into machine code. This machine code is executed right away because of the JIT compilation of JS. The machine code that is generated initially is a very un-optimised version of the code just so that the execution can begin as soon as possible. In the background, the machine code is taken and recompiled while the program is already executing. During each iteration of the optimisation, the unoptimised code is swapped for the optimised code - all while the program is already executing!!
    
    ![[Untitled 6.png]]
    
    A JS Runtime is something that is required to run JS in the browser. It is a combination of JS Engine (discussed above), the Web APIs, and the Callback Queue. The Callback Queue is a datastructure that contains all the callback functions that are ready to be executed. What are callback functions? The functions that you attached to the event listeners are known as callback functions. So when an event happens - like a click - the corresponding callback function will be called.
    
    ![[Untitled 7.png]]
    
    - This is how it happens - when a user clicks on a button, the callback function is added to the callback queue. Then, once the call stack is _empty_, the callback function is passed to the call stack so that it can be executed. This happens by something known as the Event Loop. The event loop picks up functions from the callback queue and places them in the call stack so that they can be executed. The event loop is basically how javascript's non-blocking concurrency model is implemented.
    
    ![[Untitled 8.png]]
    
    Remember that JS can also exist outside of browsers (nodeJS). In that case, the JS runtime will not contain the Web APIs, because the Web APIs are provided by the browser. In this case, the WebAPIs are replaced by C++ bindings and the thread pool.
    

## Lecture 91: Execution Contexts and the Call Stack

- Once the code is code is compiled and ready to be executed, a _Global Execution Context_ is created for the top level code. Top-level code here means any code that is not inside any function.For example, the _name_ variable is a part of the top-level code. Hence it will be executed in the Global execution Context. Then we have two functions - one expression and one declaration. These functions will be declared so that they can be called later. The point is that - the code inside the function will be executed only when the function is called.An _Execution Context_ is defined as an environment in which a piece of JS code is executed. The context stores all the necessary information that is required by a code to be executed - such as local variables or arguments passed to a function.In every JS project, there is _only_ one global execution context and this global execution context is where the top level code executes.

![[Untitled 9.png]]

- Now that we have a top-level execution context, we can begin execution of the code. For each function in the code, a _new_ execution context is created that contains all the necessary information that that particular function requires to run. The same also goes for _methods_ because they are just functions attached to objects. All these execution contexts together make up the call stack. Once all the functions have been executed, the JS Engine will wait for the callback function to arrive so that it can begin executing those.

![[Untitled 10.png]]

## Lecture 92: Scope and the Scope Chain

- _Scope_ controls where you can access a particular variable and where you can't access them.JS uses _Lexical Scoping_, which means that scoping is controlled by the physical placement of functions and blocks in the code.In JS, we have 3 types of scope: _global_ scope, _function_ scope, and _block_ scope.Note that only variables declared with `let` or `const` are restricted to the block in which they are created. That means that a variable that is declared using a `var` would be scoped to the parent function, or else the global scope. Hence, we say that `let` and `const` are _block scoped_ whereas `var` is `function scoped`. IN ES5 and earlier versions, we only had global scope and function scope.

![[Untitled 11.png]]

## What is the Scope Chain?

  

- Each function creates it's own scope. Hence, _first()_and _second()_ get their own scopes.
- Every scope has access to all of the variables from all of its outer/parent scopes. All of this also applies to function `arguments`. So this is basically how the Scope Chain works - if one scope needs to use a variable but is unable to find it in the current scope, it will look up it's scope chain and see if it can find the variable in one of it's parent scopes. If it can, then it will use that variable. If it can't, then you get an error. This process is also called _variable lookup in scope chain_. Note that a scope cannot look for variables in it's children scopes.
- - Note that starting from ES6, the if-block is also able to create it's own scope. But this scope will only work for the ES6 variable types - `let` and `const`. - Take note of the _millenial_ variable in the below figure. The variable is declared with the `var` keyword. Hence, it is not scoped to the block, but rather scoped to the parent function, which in this case is _first()_. Hence, unlike the _decade_ variable, we see the _millenial_ variable in the scope of the _first()_ function.

![[Untitled 12.png]]

- What is the difference between the Scope Chain and the Call Stack?
- The scope of a function is the same as the Variable Environment in the function's Execution Context. Apart from this, the scope also inherits the scope from all of it's parent scopes because of the scope chain that we discussed above.
- The order of function calls has no effect on the scope chain. What I mean by this is, consider the following:
- h

![[Untitled 13.png]]

```JavaScript
'use strict';

// function declaration
function calcAge(currYear, birthYear) {
    console.log(`First Name: ${firstName}`);
    let age = currYear - birthYear;

    function logMessage() {
        let output = `${firstName}, you are ${age} years old, and were born in ${birthYear}.`;
        console.log(output);

        if (birthYear >= 1981 && birthYear <= 1996) {
            var millennial = true;
            let str = `You are also a millennial, ${firstName}`;
            console.log(str);

            function concatenateStrings(str1, str2) {
                return str1 + str2;
            }
        }

        // Validate that variables declared 'let' and 'const' are block-scoped
        // This will produce a reference error, because const and let variables are block-scoped.
        // So they are available only inside the block in which they were created:
        // Uncaught ReferenceError: str is not defined
        // console.log(str);

        // Validate that variables declared 'var' are function-scoped
        // But note that the millennial variable is in scope.
        // Because it was declared as a var. And varibales declared with var are function-scoped.
        // i.e. they ignore the block in which they were declared and instead take the scope of their parent function instead.
        if (millennial) console.log(`Are you happy now?`);

        // Validate that functions are block-scoped
        // The following line now produces an error. Because the scope of the concatenateStrings function is the
        // block in which it was defined. But remember that this is valid only for 'strict mode'.
        // If you comment out the 'strict' at the top of the code, then this function will be within scope!!
        // Uncaught ReferenceError: concatenateStrings is not defined
        concatenateStrings('Hello', 'World');
    }

    logMessage();

    return age;
}

const firstName = "Alice";
calcAge(2021, 1990);
```

## Lecture 94: Variable Environment - Hoisting and the TDZ

- Hoisting: makes some types of variables accessible/usable in the code before they are actually declared. In other words, variables are "lifted" to the top of their scope. Behind the scenes, what actually happens is that, before execution, the code is scanned for variable declarations and for each variable a new property is created in the _variable environment object_The practical implementations of hoisting is that you can use a variable in the code _BEFORE_ it is actually declared! The value associated with the hoisted variables also varies - refer the table below.

![[Untitled 14.png]]

function declarations are hoisted - this means that these types of functions can be used/called before they are declared.

Variables declared with `var` are also hoisted, but the hoisting works differently when compared to functions in the sense that the variables are hoisted, but their value is `undefined` and not the value that they are declared with.

`let` and `const` variables are not hoisted. Hence these variables cannot be used before they are declared. TDZ stands for the Temporal Dead Zone, which is indicating the region in the code where the variable is in-scope but cannot be used because it has not been declared yet.

- For function expressions and arrow functions, the hoisting depends on whether they were created using `var`, `const`, or `let`. What this means is that function expressions and arrow functions declared with a `var` is hoisted, but to `undefined`. But if they have been declared using `let` or `const`, the functions are not usable before they have been declared in the code - because of the TDZ.

- There is one another difference between variables declared with a `var` and `let`/ `const` - the variables declared with `var` are added as properties to the global `window` object in JS, whereas the variables declared with `let`/`const` are not.

![[Untitled 15.png]]

## What is Inside an Execution Context

- What is inside an Execution Context:
- **Variable Environment**: This environment stores all the variables and the function declarations. Apart from this, it also stores a special object called the `arguments` object. This object contains all the arguments that were passed into the function that the current Execution Context belongs to. Remember that each function gets its own execution context as soon as the function is called.
- **Scope Chain**: Apart from the local variables defined inside the function, the Execution Context can also access variables that are defined outside the function. The scope chain consists of _references_ to variables that are outside the current function.
- `this` **keyword** : One important sidenote is that Execution Contexts belonging to the arrow functions do not get their own `this` keyword or `arguments` object. Instead, they use the `this` and `arguments` of their nearest normal function parent.
- An example explaining all of this can be found [here on Udemy Lecture](https://www.udemy.com/course/the-complete-javascript-course/learn/lecture/22648479?start=488)
- Recall, as we discussed earlier, the Call Stack with the Memory Heap forms the JS Engine. The Call Stack is the place where the Execution Contexts get stacked on top of each other to keep track of where we are in the execution. The Execution Context on the top of the stack is the one that is currently running. [This Udemy Lecture](https://www.udemy.com/course/the-complete-javascript-course/learn/lecture/22648479?start=488#notes)
- explains using a code example how JS makes use of the Call Stack and the Execution Context to execute the code.

![[Untitled 16.png]]

  

## Lecture 96: The this keyword

- The `this` keyword is a special variable that is created for every Execution Context (i.e. for each function). We saw earlier that the `this` keyword is one of the three things that are a part of the Execution Context that is created for every function. In general terms, the `this` keyword takes the value of the "owner" of the function in which the `this` keyword is used.The value of `this` is not static and varies depending on the way that the function is called.A function can be called in multiple ways.A function can be a method - i.e. a function that is present on a object. When we use the `this` keyword in a method, the `this` keyword points to the object calling the method.A function can be called separately - like a usual stand-alone function call. In that case, the `this` keyword is `undefined`. But this is only in the case when we are using the "strict" mode. If we are not using the strict mode, tehn in that case the `this` keyword is going to point at the global object, which in this case is the `window` object.Arrow functions are not a way to call functions but they still need to be considered. Arrow functions do not get their own `this` keyword. Instead, if you use the `this` keyword in an arrow function, it is just going to be the `this` keyword of the surrounding/parent function.If the function is called as an event listener, then the `this` keyword will point to the DOM Element that the handler function is attached to.

```JavaScript
// Lecture 97: The this keyword in practice

// The this keyword in the global scope is simply the 'window' object
console.log(this); // Prints: Window

// 'this' keyword when we are using a simple function call
const calculateAge = function (currYear, birthYear) {
    console.log(this); // Prints: undefined. Because calculateAge method is being called as a simple function call.
    return birthYear - currYear;
}
calculateAge(2020, 1990);

// 'this' keyword when we are using a function call for an arrow function
// Recall arrow functions do not get their own 'this' keyword.
// Instead it refers to the 'this' of the parent function/scope
// In this case the parent function/scope is just the global scope, and hence we get window
const calculateAgeArrow = (currYear, birthYear) => {
    console.log(this); // Prints: Window. Because 'this' in arrow function refers to the parent function, which is the global scope in this case.
    return birthYear - currYear;
}
calculateAgeArrow(2020, 1990);

// 'this' keyword when we are using a method i.e. a function attached to a object
// When we use the 'this' keyword inside of a method, then the 'this' keyword refers to the object that is calling the method
// In this case, it is the dave object
const dave = {
    birthYear: 1991,
    calcAgeDave: function (currYear) {
        console.log(this); // Prints: 'dave'. Because 'this' refers to the dave object
        console.log(currYear - this.birthYear); // Hence we can use 'this' to reference the properties in the object
    }
}
dave.calcAgeDave(2021);

// Note that the 'this' keyword points to the object calling the method,
// not the object in which we wrote the method
// Let us see an example by doing this:
const eve = {
    birthYear: 2000
};
// Since functions are just variables, we can write this
// This will add a new property to the eve object that will contain the calcAgeDave function from the dave object
// This is often called: Method Borrowing
eve.calcAgeEve = dave.calcAgeDave;
// So now let's call the method calcAge. What do you think the 'this' will refer to?
// It refers to the eve object now.
// This shows that the 'this' keyword points to the object that is calling the method, and not the object that contains the method
eve.calcAgeEve(2021);

// We can go even one step further, and just take the function completely out of the object
// Now the 'this' keyword prints 'undefined' because we are making a simple function call
// and we know that in those, the 'this' keyword is just 'undefined'
const myFunc = dave.calcAgeDave;
myFunc(2021); // Prints: undefined
```

![[Untitled 17.png]]

  

  

## Lecture 98: Regular Functions vs Arrow Functions

- One important point is that arrow functions should never be used as methods on an object. You should always use function expressions. This is to do with the fact that arrow functions do not get their own `this` keyword, instead they inherit the `this` keyword from their parent scope. This can be a source of bugs.

```JavaScript
// Consider another object that uses an arrow function instead of the
// function expression that was used above
const felicia = {
    firstName: 'Felicia',

    // Avoid using arrow functions as methods since they can lead to errors
    greet: () => {
        console.log(this);
        console.log(`Hi ${this.firstName}`);
    },

    // Prefer function expressions as methods instead
    greet2: function () {
        console.log(this); // Now 'this' refers to the object calling the method, which is felicia
        console.log(`Hi ${this.firstName}`);
    }

}
// Like we said, an arrow function does not get it's own 'this' keyword
// instead it adopts the 'this' keyword of it's enclosing scope
// Note that even though the function is defined inside an object, the object is just an object literal ie.
// the object does not get it's own scope. Hence the enclosing scope of the greet function is still the global/window object
// Hence 'this' will refer to the global/window object
// And since there is no 'firstName' property on the window object, `Hi ${this.firstName}` will print undefined.
felicia.greet(); // Prints: undefined
felicia.greet2(); // Prints: 'Hi Felicia'
```

- Another important point is when we have to use functions inside methods.
- Consider the following function inside a method. Why does line 11 print undefined? If you look closely, you can see that the _isMillennial_ function is being called as a simple function call. So, even though the function is within a method, it is still a regular function call. And the rule says that inside a normal function call, the `this` keyword is `undefined`.

  

```JavaScript
const gina = {
    firstName: 'Gina',
    birthYear: 1990,

    greet: function () {
        console.log(this); // Prints the object gina
        console.log(`Hi ${this.firstName}`);

        // This is a function expression inside a method
        const isMillenial = function () {
            console.log(this); // Prints: undefined
            // Now this produces an error:
            // Uncaught TypeError: Cannot read property 'birthYear' of undefined
            if (this.birthYear >= 1981 && this.birthYear <= 1996) {
                console.log(`You are also a Millennial!`);
            }
        }

        isMillenial();
    }
}
gina.greet();
```

One way to get around this problem in solutions prior to ES6 was as follows:

```JavaScript
const gina2 = {
    firstName: 'Gina',
    birthYear: 1990,

    greet: function () {
        console.log(this); // Prints the object gina
        console.log(`Hi ${this.firstName}`);

        const self = this; // sometimes the variable 'that' is also used in place of 'self'
        const isMillenial = function () {
            console.log(self); // Prints the object gina
            if (self.birthYear >= 1981 && self.birthYear <= 1996) {
                console.log(`You are also a Millennial!`);
            }
        }
        isMillenial();
    }
}
gina2.greet();
```

- However, starting form ES6, we can use the arrow function to get around this problem. Recall that the arrow function does not have the `this` keyword of its own and instead inherits the `this` keyword of its parent scope. So, we can do this:

- TODO: but we are still calling the _isMillennial()_ functio as a normal function inside the _gina3_ object. Then shouldn't the `this` keyword be `undefined`?

```JavaScript
const gina3 = {
    firstName: 'Gina',
    birthYear: 1990,

    greet: function () {
        console.log(this); // Prints the object gina3
        console.log(`Hi ${this.firstName}`);

        const isMillenial = () => {
            console.log(this); // Prints the object gina3. Because this is inside the arrow function that inherits the parent scope
            if (this.birthYear >= 1981 && this.birthYear <= 1996) {
                console.log(`You are also a Millennial!`);
            }
        }
        isMillenial();
    }
}
gina3.greet();
```

- Another thing is the `arguments` variable that we discussed earlier:

```JavaScript
// arguments Keyword
const sumOfNumbers = function (a, b) {
    console.log(arguments); // This logs the args that are passed in into the function - 10, 20
    return a+b;
}
sumOfNumbers(10, 20);

// Interestingly enough, this function can also be called with more than two parameters
const anotherSummation = function (a, b) {
    console.log(arguments);
    // 'arguments' is an array. Hence we could cycle through each element and do computations on the arguments if required
    return a + b;
};
anotherSummation(2,3,4,5,6);

// However, the arrow function does nto get it's own arguments keyword. Just like the 'this' keyword
const arrowSummation = (a, b) => {
    console.log(arguments); // Uncaught ReferenceError: arguments is not defined
    return a + b;
};
arrowSummation(2,3,4);
```

## Lecture 99: Primitives vs Reference Types

![[Untitled 18.png]]

## Understanding Memory allocation in JavaScript

1. All the primitives are stored in the stack. All the Reference datatypes are stored in heap.

Stack--> Primitives--> (String,Number,Boolean,null,undefined,sybol,Bigint)

Heap-->Reference--> (Objects, Arrays, Functions)

1. Whenever a copy of the Stack Primitive variable is made then a literal copy is created but in (Non primitive)variables a reference to the object in the heap is created.

![[Untitled 19.png]]

![[Untitled 20.png]]

![[Untitled 21.png]]

So if I make a change for userTwo userOne contents are also changed