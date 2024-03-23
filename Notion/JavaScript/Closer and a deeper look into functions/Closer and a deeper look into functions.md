[](https://www.notion.soundefined)

# Lecture 126: Default Parameters

- ES6 introduced Default Parameters

```JavaScript
// Consider the following code where we are creating a new
const bookings = [];

const createBooking = function (flightNum, numPassengers, price) {

    // We are creating a new object using the enhanced literal syntax
    // where we do not have to specify key value pairs like:
    // const newBooking = {
    //     flightNum: flightNum,
    //     numPassengers: numPassengers,
    //     price: price
    // }

    const newBooking = {
        flightNum,
        numPassengers,
        price
    }
    console.log(newBooking);
    bookings.push(newBooking);
}

// Recall that we can call the createBooking() function with lesser num of args
createBooking('LH123'); // Prints: {flightNum: "LH123", numPassengers: undefined, price: undefined}

// What we would like is to use default values when no parameters are specified for the args
// We add default params by changing the function declaration
const createBookingNew = function (flightNum, numPassengers = 1, price = 199.99) {

    const newBooking = {
        flightNum,
        numPassengers,
        price
    }
    console.log(newBooking);
    bookings.push(newBooking);
}

createBookingNew('LH123'); // Prints: {flightNum: "LH123", numPassengers: 1, price: 199.99}
// An obviously we can override these defaults
createBookingNew('LH123', 2, 400); // Prints: {flightNum: "LH123", numPassengers: 2, price: 400}

// But another thing that we can do with default parameters is use the value of the previously supplied params to calculate them
// Note how we are using the value of the numPassengers to calculate the price
const createBookingNew2 = function (flightNum, numPassengers = 1, price = 199.99 * numPassengers) {

    const newBooking = {
        flightNum,
        numPassengers,
        price
    }
    console.log(newBooking);
    bookings.push(newBooking);
}
// The price is being calculated using the args passed in.
createBookingNew2('LH123', 10); // Prints: {flightNum: "LH123", numPassengers: 10, price: 1999.9}

// One thing to note is that you cannot use a param in the default value calc befor the param has been defined
// So you cannot do this:
const createBookingNew3 = function (flightNum, price = 199.99 * numPassengers, numPassengers = 1 ) {

    const newBooking = {
        flightNum,
        numPassengers,
        price
    }
    console.log(newBooking);
    bookings.push(newBooking);
}

// Note we are passing in 'undefined' as the param to the second arg. This is the same as not passing in an arg.
// Uncaught ReferenceError: Cannot access 'numPassengers' before initialization
createBookingNew3('LH123', undefined, 5); // Error
```

  

## Lecture 127: How passing arguments works - Value vs. Reference

  

- In JS, Primitives are passed by value, and Objects are passed by "copy of a reference".

[Source on SO here](https://stackoverflow.com/questions/13104494/does-javascript-pass-by-reference)

  

```JavaScript
const flight = 'LH234';
const jonas = {
    name: 'Jonas Schmedtmann',
    passport: 24739479284,
};

const checkIn = function (flightNum, passenger) {
    // We are changing the flightNum
    flightNum = 'LH999';
    // And we are also changing the name of the passenger
    passenger.name = 'Mr. ' + passenger.name;

    if (passenger.passport === 24739479284) {
        console.log('Checked in');
    } else {
        console.log('Wrong passport!');
    }
};

checkIn(flight, jonas);
// Note that the flight remains unchanged because the value was copied
console.log(flight); // Prints: LH234
// But the name changes, because the 'name' field is within an object, and objects are passed by reference
// (well the value of their reference is copied and passed in by value)
// So JS only has pass-by-value.
// This is in contrast to other languages like C++ where the primitives can also be passed by their reference
console.log(jonas); // Prints: {name: "Mr. Jonas Schmedtmann", passport: 24739479284}

const newPassport = function(person){
person.passport = Math.trunc(Math.random()*1000000);
};
newPassport(jonas);
checkIn(flight,jonas);
```

In JavaScript, primitive values are passed by value, and objects are passed by a "copy of a reference". This means that while a reference to an object is passed, the reference itself is still a value that contains a memory address. Essentially, we pass a reference, but we do not pass by reference.

In contrast to languages like C++, where primitives can also be passed by their reference, JavaScript only has pass-by-value. While we do pass a reference for objects in JavaScript, the reference itself is still a value that contains a memory address. Essentially, we pass a reference, but we do not pass by reference.

![[Untitled 21.png]]

A copy is created and pointed to the heap

[![](https://rgbk21.github.io/Code/JavaScript/imgs/chevron-expand.svg)](https://rgbk21.github.io/Code/JavaScript/imgs/chevron-expand.svg)

  

**First Class vs Higher order function**

  

## Lecture 128: First-Class and Higher-Order Functions

JS has First-Class functions that enables us to write Higher-Order functions. But what does this mean?

- What does JS having 'First-Class Functions' mean?
- It just means that functions are a different 'type' of object in JS. Since objects are values, functions are values too. And since functions are values, they can be stored in variables or even object properties (keys in an object). We have already been doing this so far.
- - We can also pass functions as args to other functions. We already did that with `addEvenListener` where we passed in the callback function as an argument .
- We can also return a function from another function
- Finally, since functions are objects, and objects can have methods on them, there can also be function methods. `bind` method is an example of that.

- What is a Higher-Order Function then?
- A Higher-Order function is either a function that receives a function as an argument, or returns a new function, or both.

`addEvenListener` is an example of a higher-order function because it accepts another function as an arg.

- Similarly we can have functions that return other functions.

![[Images-attachments/Untitled 23.png|Untitled 23.png]]

![[Images-attachments/Untitled 1 3.png|Untitled 1 3.png]]

```JavaScript
// Two functions that will passed into the higher-order fn as args
const oneWord = function (str) {
    return str.replace(/ /g, '').toLowerCase();
};

const upperFirstWord = function (str) {
    const [first, ...others] = str.split(' ');
    return [first.toUpperCase(), ...others].join(' ');
};

// This is a higher-order function:
// because it is a functions accepting callback functions
const transformer = function (str, fn) {
    console.log(`Original string: ${str}`);

    // Note how we are calling the function
    const returnedVal = fn(str);
    console.log(`Transformed string: ${returnedVal}`);
    // Transformed string: JAVASCRIPT is the best!
    // Transformed string: javascriptisthebest!

    // Since functions are objects, functions can have methods of their own. Functions can even have properties
    // fn.name is one such property that returns the name of the function
    console.log(`Transformed by: ${fn.name}`);
    // Transformed by: upperFirstWord
    // Transformed by: oneWord

};

// We call the higher-order fn as follows:
transformer('JavaScript is the best!', upperFirstWord);
transformer('JavaScript is the best!', oneWord);

// JS uses callbacks all the time.
const high5 = function () {
    console.log('👋');
};
// Here, the addEventListener() is just like the transformer() function we wrote above
// and the high5 function is the callback function like the oneWord/upperFirstWord function
// On it's own, the addEventListener would have no idea what do do when a 'click' event occurred.
// It is the callback function that actually executes the required functionality.
// Hence callback functions are described as a means of implementing abstractions. Where addEventListener is more of a 
// 'higher-order' (hence the name) of abstraction
// and the high5() method is more of a lower level of abstraction.
document.body.addEventListener('click', high5);
```

## Lecture 130: Functions accepting Callback Function s

```JavaScript
// Functions Returning Functions
const greet = function (greeting) {
    return function (name) {
        console.log(`${greeting} ${name}`);
    };
};

// Since 'greet' returns a function, 'greeterhey' is also a function
const greeterHey = greet('Hey');

// And hence we can call 'greeterHey' with args of its own
greeterHey('Alice'); // Prints: Hey Alice
greeterHey('Bob'); // Prints: Hey Bob

// And we can also do something like this.
// greet('Hello') returns a function in which we then pass 'Charlie'
greet('Hello')('Charlie'); // Prints: Hello Charlie

// You can even rewrite the greet() function using just the arrow notation
const greetArrow = greeting => name => console.log(`${greeting} ${name}`);

const greetArrowFn = greetArrow('Hey there');
greetArrowFn('Dave'); // Prints: Hey there Dave 
```

## Lecture 131: The call and apply methods

- We can set the `this` keyword manually in JS as well.

```JavaScript
// Recall how the 'this' keyword worked in the case of simple objects

const alice = {
    name: 'Alice Doe',
    age: 21,
    birthYear: 1990,
    friends: [],
    addFriend(newFriend) {
        // The 'this' keyword will point to the object which called this method
        this.friends.push(newFriend);
        console.log(`${this.name} made a new friend named ${newFriend}`);
    },
    addFriendAndAge(newFriend, age) {
        this.friends.push(newFriend);
        console.log(`${this.name} made a new friend named ${newFriend} whose age is ${age}`);
    }
}

alice.addFriend('Bob Doe');

// Now suppose we wanted to create a new person named Bob.
// Bob would have the same property names as Alice.
// It would also have a addFriend() method.
// But we do not want to duplicate the method again for the Bob object.
const bob = {
    name: 'Bob Doe',
    age: 22,
    birthYear: 1989,
    friends: []
}

// Instead, what we do is we store the method from the alice object in an external variable and then reuse the function
const addNewFriendExt = alice.addFriend;

// Remember, if we tried to call the addNewFriendExt function here, we would have an error because this is an external function
// and like we had seen earlier, for a regular function call, the 'this' keyword is 'undefined'
addNewFriendExt('Alice'); // Uncaught TypeError: Cannot read property 'friends' of undefined

// So now the question is
// how do we tell JS explicitly which object it should refer to when using the addNewFriendExt() method?
// The requirement is: if we want to add a friend to the alice object, the 'this' keyword should refer to the 'alice' object
// and if we want to add a friend to the bob object, the 'this' keyword should refer to the 'bob' object

// There are 3 options: call, apply, bind

// 1) call

// Recall that functions are objects, and objects have methods.
// Hence we can access the 'call' method on a function
// The first argument in the call method is object that we want the 'this' keyword to point to.
// All the args after the first one are the args of the original function on which we are calling the 'call' method
addNewFriendExt.call(alice, 'Charlie Doe');
addNewFriendExt.call(bob, 'Dave Doe');

console.log(alice.friends); // Prints: ["Bob Doe", "Charlie Doe"]
console.log(bob.friends); // Prints: ["Dave Doe"]

// Note this method: addNewFriendExt.call(bob, 'Dave Doe');
// So the important point to consider is, even though we had the 'this' keyword inside the 'alice' object,
// we still managed to manually override the 'this' keyword by calling the function using the 'call' method and passing
// in our own 'this' variable

// Similar we can do this for any other object
const charlie = {
    name: 'Charlie Doe',
    age: 23,
    birthYear: 1988,
    friends: []
}
addNewFriendExt.call(charlie, 'Alice Doe'); // Prints: Charlie Doe made a new friend named Alice Doe
console.log(charlie.friends); // Prints: ["Alice Doe"]

// 2) apply

// This is similar to the call method we discussed above, except that the args to the function are provided as an array
const addFriendAndAgeExt = alice.addFriendAndAge;
addFriendAndAgeExt.call(charlie, 'Bob Doe', 21); // Prints: Charlie Doe made a new friend named Bob Doe whose age is 21
// Using apply, it would be:
addFriendAndAgeExt.apply(charlie, ['Dave Doe', 22]); // Prints: Charlie Doe made a new friend named Dave Doe whose age is 22

// But we avoid using this, as we can do just this:
const friendDetails = ['Eve Doe', 21];
addFriendAndAgeExt.call(charlie, ...friendDetails); // Prints: Charlie Doe made a new friend named Eve Doe whose age is 21
```

  

## Lecture 132: The bind method

- Just like the `call` and the `apply` method, the `bind` method allows us to set the `this` keyword for any function call.The difference is that, unlike the `call` and `apply` methods, `bind` does not immediately call the function. Instead it returns a NEW function where the `this` keyword is already bound.This is especially useful when we have to manually assign the `this` keyword in the case of event listeners. Recall for event listeners the callback function should be a function, and it should not be immediately called. `call` and `apply` immediately call the function. Hence we use the `bind` method instead.

```JavaScript
// Just like the 'call' and the 'apply' method, the 'bind' method allows us to set the 'this' keyword for any function call
// The difference is that, unlike the call and apply methods, bind does not immediately call the function.
// Instead, it returns a new function where the 'this' keyword is bound.
const dave = {
    name: 'Dave Doe',
    age: 23,
    birthYear: 1988,
    friends: [],

    addFriendAndGender(newFriend, gender) {
        this.friends.push(newFriend);
        console.log(`${this.name} made a new friend named ${newFriend} with gender ${gender}`);
    }
};

const addAnotherFriend = dave.addFriendAndGender;

const eve = {
    name: 'Eve Doe',
    age: 23,
    birthYear: 1988,
    friends: []
}
// We are using the bind method to set 'this' keyword to eve.
// Note that this will not call the 'addAnotherFriend' function.
// Instead it will return a new function where the 'this' keyword will always be set 'eve'
const addFriendForEve = addAnotherFriend.bind(eve);

// And we can call this function like:
addFriendForEve('Felicia', 'female');
// Prints: Eve Doe made a new friend named Felicia with gender female
// We no longer have to specify the 'this' keyword separately now!
// And we can test this:
console.log(eve.friends); // Prints: ["Felicia"]

// Notice how in the 'call' method, apart from the 'this', we could pass in args for the functions.
// We can do the same for 'bind' method as well.
// Now in this case, the name of the friend is fixed as 'Alex'
const addFriendsForEveNamedAlex = addAnotherFriend.bind(eve, 'Alex');
// And we call the function as follows, passing ONLY the second args now.
addFriendsForEveNamedAlex('female'); // Prints: Eve Doe made a new friend named Alex with gender female

// This kind of technique where we pre-set a part of the args beforehand is known as 'Partial Application'.

// Using the bind technique with Event Listeners
const lufthansa = {
    airline: 'Lufthansa',
    iataCode: 'LH',
    bookings: [],
    // book: function() {}
    book(flightNum, name) {
        console.log(
            `${name} booked a seat on ${this.airline} flight ${this.iataCode}${flightNum}`
        );
        this.bookings.push({ flight: `${this.iataCode}${flightNum}`, name });
    },
};

// Add a new property to the lufthansa object called 'planes'. This means that the company currently has 300 planes
lufthansa.planes = 300;
// Whenever you buy a plane, your number of planes is incremented by 1
lufthansa.buyPlane = function () {
    console.log(this);
    this.planes++;
    console.log(this.planes);
}
// So the intention is that whenever a user clicks on the 'Buy Planes' button, the buyPlane method should be called and increment
// the count of planes by 1 and log it to the console.
// But now, when we click on the button, the 'this' keyword logs: <button class="buy">Buy new plane 🛩</button>
// document.querySelector('.buy').addEventListener('click', lufthansa.buyPlane);


// Why does the 'this' keyword log the element, and not the 'lufthansa' object.
// In L96 we read that:
// If the function is called as an event listener, then the 'this' keyword will point to the DOM Element that the handler function is attached to.
// So this is expected behavior.
// In order to get around this problem, we now have to manually assign the 'this' keyword to this function so that we get the intended behavior.
// But which way should we follow? Should we use call, apply, or bind.
// ONLY bind allows us to manually assign the 'this' keyword without immediately calling the function.
// And since this is a callback function, that is what we want to happen
// Hence we use the 'bind' method in this case
// And we replace the above even listener with the following:
document.querySelector('.buy').addEventListener('click', lufthansa.buyPlane.bind(lufthansa));
// And now we get the correct behavior
// And we see that the 'this' logs:
// {airline: "Lufthansa", iataCode: "LH", bookings: Array(0), planes: 300, book: ƒ...}
// which is what we intended
```

## Lecture 134: Immediately Invoked Function Expressions

- Sometimes in JS, we need a function that is only executed once, and never executed again. We need this to implement something known as async await.
- it is basically used to prevent pollution from global scope
- arrow function can also be used for **Immediately Invoked Function**

```JavaScript
// We transform the function statement that we have into an expression by wrapping it in braces - '()'
// And then we call the function immediately by appending the ()
// This pattern is known as the Immediately Invoked Function Expressions (IIFE)
(
    function () {
        console.log('This method will be executed only once');
    }
)(); // Prints: This method will be executed only once

// The same can be done with arrow functions as well
const v = () => console.log('Inside the arrow function');
v(); // This can be called any number of times that you want

// Remove the variable assignment, convert it into an expression, call it immediately
(
    () => console.log('Inside the arrow function that can only be called once')
)(); // Prints: Inside the arrow function that can only be called once

// Just fiddling around to see if this works. And it does!
(console.log)('Does this work'); //  Prints: Does this work

// Why was this done?
// So that we can avoid overwriting our local variables by global variables that may be defined having the same name
// For example:
(
    function () {
        console.log('This method will be executed only once');
        const privateVariable = 21;
    }
)();

// The privateVariable declared above will not be accessible over here:
console.log(privateVariable); // Uncaught ReferenceError: privateVariable is not defined
// And hence, this pattern can be used to perform encapsulation

// If your concern is just protecting scopes, then you do not need to follow the IFFE pattern.
// You can just do this:
{
    const oneName = 'Ein name';
    let twoName = 'Du name';
    var threeName = 'Three name';
}

console.log(oneName); // Uncaught ReferenceError: oneName is not defined
console.log(twoName); // Uncaught ReferenceError: twoName is not defined
console.log(threeName); // Prints: Three name
```

  

## Important:—>Closures

First Understand the Scope chain and the call Stack

When a function is called it is inserted into the call stack and the scope chain of it is also decided.

  

![[Images-attachments/Untitled 2 2.png|Untitled 2 2.png]]

After the `secureBooking()` is executed:

![[Images-attachments/Untitled 3 2.png|Untitled 3 2.png]]

  

Now you might be questioning what will happen if I now run the `booker()` function because it is supposed to increment `passengerCount` , but the variable and `secureBooking()` is out of scope chain and call stack.so it should not be possible to access the passenger count but but but observe the next immage

```JavaScript
// Consider the following function that returns a function
const secureBooking = function () {
    let numOfPassengers = 0;
    return function () {
        numOfPassengers++;
        console.log(`${numOfPassengers} number of passengers.`);
    }
}

// Now let us call this function:
const booker = secureBooking();

// And then we call booker. And then this happens:
booker(); // Prints: 1 number of passengers.
booker(); // Prints: 2 number of passengers.
booker(); // Prints: 3 number of passengers.

// So the question is:
// how can the booker function, that is defined in the global scope, access the numOfPassengers variable defined in the child scope
// of the secureBooking function? Not only access, but also store and then change the values each time it is called.
// Also, numOfPassengers is defined as a local variable in the secureBooking function.
// The normal understanding is that once a function finishes its execution, the local variables to the function can no longer be accessed.
// But in this case we can access and change the values of the variable -
// even after the function secureBooking has returned - through the booker function
```

![[Images-attachments/Untitled 4 2.png|Untitled 4 2.png]]

  

![[Images-attachments/Untitled 5 2.png|Untitled 5 2.png]]

not only it does not throw any error but it acesses the `passengerCount` and increments this how is this possible?

This is due to a concept called Closure—> Which is basically a rule that says that Any function always has access to the variable environment of the execution context in which the function was created, even after that execution context is gone.

- Read about closures [on MDN here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures#closure).

- A _closure_ is the combination of a function and the lexical environment within which that function was declared. This environment consists of any local variables that were in-scope at the time the closure was created.

![[Images-attachments/Untitled 6 2.png|Untitled 6 2.png]]

![[Images-attachments/Untitled 7 2.png|Untitled 7 2.png]]

We can access the closure of a variable by doing:

```JavaScript
// We can log the variables stored in the closure like this:
console.dir(booker);
```

And now if look into the console, you will see something like this. Note that when the name of a property is enclosed in `[[]]`, as shown below, it means that the property is an internal property that cannot be accessed by us through the code.

![[Images-attachments/Untitled 8 2.png|Untitled 8 2.png]]

````JavaScript
// Example 1: the closure of a function can be re-assigned

let f;

const g = function () {
    const a = 23;
    f = function () {
        console.log(a * 2);
    };
};

g();

// So even though the f variable has been defined outside of the g function,
// it still closes over the variable environment in the g function
f(); // Prints: 46

const h = function () {
    const b = 7;
    f = function () {
        console.log(b * 2);
    };
};

// Reassigning the f variable by calling the h() function
h();
f(); // Prints: 14
// So, in effect, the SAME f variable has been assigned to two different functions with two different variable environments


// Example 2: Showing usage of closures in a Timer.

// Note: We do not need to always return a function in order to see a closure in action.
const boardPassengers = function (n, wait) {
    const perGroup = n / 3;

    setTimeout(function () {
        console.log(`n = ${n} passengers`); // Prints: n = 180 passengers. As was passed in the fn arg. And not 1000, as is defined in the parent scope
        console.log(`There are 3 groups, each with ${perGroup} passengers`);
    }, wait * 1000);

    console.log(`Will start boarding in ${wait} seconds`);
};
boardPassengers(180, 5);
Based on the given code, here's the expected output when calling `boardPassengers(180, 5)`:

```
Will start boarding in 5 seconds
```

After a 5-second delay:

```
n = 180 passengers
There are 3 groups, each with 60 passengers
```
Even  after boardPassengers is executed setTimeout has acess to Board passengers variables like n and wait;
The first log statement `"Will start boarding in 5 seconds"` is executed immediately because it is not inside the `setTimeout` callback.

The second log statement is inside the `setTimeout` callback function, which is executed after the specified delay of 5 seconds. It logs the values of `n` and `perGroup` from the parent scope, resulting in the output `n = 180 passengers` and `There are 3 groups, each with 60 passengers`.

// Showing that the variables declared in the closure take priority over the variables declared in the parent scope
const perGroup = 1000;

// Example 3: Using Closures in IFFE
(function () {
    const header = document.querySelector('h1');
    header.style.color = 'red';

    document.querySelector('body').addEventListener('click', function () {
        header.style.color = 'blue';
    });
})();
````