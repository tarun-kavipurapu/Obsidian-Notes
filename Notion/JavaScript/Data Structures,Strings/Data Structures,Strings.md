## Lecture 103: Destructuring Arrays

  

- Destructuring is a way of unpacking data from an array or an object into separate variables.

```JavaScript
'use strict';

const restaurant = {
  name: 'Classico Italiano',
  location: 'Via Angelo Tavanti 23, Firenze, Italy',
  categories: ['Italian', 'Pizzeria', 'Vegetarian', 'Organic'],
  starterMenu: ['Focaccia', 'Bruschetta', 'Garlic Bread', 'Caprese Salad'],
  mainMenu: ['Pizza', 'Pasta', 'Risotto'],

  // This function is an example of returning two or more than two items from a single function
  order: function (starterIdx, mainIdx) {
    return [this.starterMenu[starterIdx], this.mainMenu[mainIdx]];
  }
};

// Normally you would do something like this:
const myArray = [1,2,3];
const a = myArray[0];
const b = myArray[1];
const c = myArray[2];

// But starting from ES6, you can do this in single line like:
// This means you are declaring 3 variables of type const named 'x', 'y', 'z'.
// This means you cannot later reassign x, y, or z to different values.
const [x, y, z] = myArray;
console.log(a, b, c); // Prints: 1 2 3

// Destructuring does not in any way affect the original array.
console.log(myArray); // Still Prints: [1, 2, 3]

// It is not necessary to extract the entire array into variables. Only a part of the array can be destructured.
const [first, second] = restaurant.categories;
console.log(first, second); // Prints: Italian Pizzeria

// So what if we wanted the first and the third elements instead? We just leave a space for the second element.
let [ctgry1, , ctgry3] = restaurant.categories;
console.log(ctgry1, ctgry3); // Prints: Italian Vegetarian

// Suppose you wanted to swap the values of two variables.
// For eg. in the above, you wanted to swap the 'ctgry1' and 'ctgry3' you would do something like this:
let temp = ctgry1;
ctgry1 = ctgry3;
ctgry3 = temp;
console.log(ctgry1, ctgry3); // Prints: Vegetarian Italian

// But now you can do just this:
// So what we are doing here is basically
// First we are creating a new array by using the [] operator containing the two elements ctgry1 and ctgry3
// Then we are destructuring the array and storing the two elements into the swapped variables. Make sense?
// We are not using let or const here because we are simple reassigning the values associated with the two variables.
[ctgry3, ctgry1] = [ctgry1, ctgry3];
console.log(ctgry1, ctgry3); // Prints: Italian Vegetarian

// We can have a function return an array and then destructure that array
// thus, in effect, having a function that returns multiple values
let [starterItem, mainCourseItem] = restaurant.order(1, 2);
console.log(starterItem, mainCourseItem); // Prints: Bruschetta Risotto

// We can also destructure nested arrays
const nestedArray = [1, 2, [3, 4]];
let [val1, , val3] = nestedArray;
console.log(val1, val3); // Prints: 1, [3,4]
// So if we want to s=destructure the array, we then do:
let [val0, , [nestedVal0, nestedVal1]] = nestedArray;
console.log(val0, nestedVal0, nestedVal1); // Prints: 1,3,4

// We can also set default values for the destructured array
let [p,q,r] = [2,3];
console.log(p,q,r); // Prints: 2 3 undefined
// But now we assign default values for each variable
[p=1,q=1,r=1] = [2,3];
console.log(p,q,r); // Prints: 2 3 1
```

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

## Lecture 104: Destructuring Objects

## Lecture 104: Destructuring Objects:

- Similar to destructuring arrays, we can also destructure objects.

```JavaScript
'use strict';

const restaurant = {
  name: 'Classico Italiano',
  location: 'Via Angelo Tavanti 23, Firenze, Italy',
  categories: ['Italian', 'Pizzeria', 'Vegetarian', 'Organic'],
  starterMenu: ['Focaccia', 'Bruschetta', 'Garlic Bread', 'Caprese Salad'],
  mainMenu: ['Pizza', 'Pasta', 'Risotto'],

  // This function is an example of returning two or more than two items from a single function
  order: function (starterIdx, mainIdx) {
    return [this.starterMenu[starterIdx], this.mainMenu[mainIdx]];
  },

  openingHours: {
    thu: {
      open: 12,
      close: 22,
    },
    fri: {
      open: 11,
      close: 23,
    },
    sat: {
      open: 0, // Open 24 hours
      close: 24,
    },
  },
};


// The variables names should exactly match the property names that we want to retrieve from the object
// Unlike destructuring in arrays, we do not need to specify all the properties
// and the properties can also be specified out-of-order.
// Just like arrays, this now creates 3 new variables
const {name, openingHours, categories} = restaurant;
console.log(name, openingHours, categories); // Prints the values from the restaurant object

// We can also change the variable names to be different from the property names
const {name: restaurantName, openingHours: workingHours, categories: tags} = restaurant;
console.log(restaurantName, workingHours, tags);

// Using Default Values

// We can also have default values in case the property that we are trying to read does not exist on an object
// The point we are trying to make here is that:
// If a property does not exist on an object, then we would get 'undefined' as is the case when defining a variable named 'menu'
// However, if a property is not defined, and we have assigned a default value to it, the variable will take the default value instead
// as is the case with the variable 'originalMenu'
// 'starterMenu' is a valid field on the object, and we are changing it's variable name to 'starters'. Hence the default value will not be taken.
const {menu, originalMenu = [], starterMenu: starters = []} = restaurant;
console.log(menu, originalMenu, starters); // Prints: undefined, [], ['Focaccia', 'Bruschetta', 'Garlic Bread', 'Caprese Salad']

// Mutating Variables

// Consider the following example
let amt1 = 100;
let amt2 = 200;
const myObject = {amt1: 1, amt2: 2};

// Now if we try to mutate the variables amt1 and amt2, we will get an error
// {amt1, amt2} = myObject;
// Hence, instead, we have to do this:
({amt1, amt2} = myObject);
console.log(amt1, amt2); // Prints: 1 2

// Dealing with Nested Objects

// Consider the following object
const {fri} = workingHours;
console.log(fri); // Prints: {open: 11, close: 23}
// Now what is we wanted to destructure the object in one line. We will make use of nested objects.
const {fri: {open, close}} = workingHours;
console.log(open, close); // Prints: 11 23

// An application of destructuring objects is when passing in parameters to functions.
// In JS, you can have many parameters being passed into a function, which can make it prone to errors if you miss a parameter
// or change the order of teh parameters or something.
// What you can do instead is box the parameters into an object, that way you can use only the parameters that you need
// and also not mess up the code in case the order changes

// Consider this function
const printPersonDetails = function (name, dob, birthPlace, job, hobbies) {
  console.log(`Hi ${name}, as per our details you are born in ${birthPlace} and have a job as a ${job}.`);
}

// Instead you could pass in an object
const printPersonDetailsUsingObj = function (person) {
  console.log(`Hi ${person.name}, as per our details you are born in ${person.birthPlace} and have a job as a ${person.job}.`);
}

// And then you can destructure the object right in the function call itself
const printPersonDetailsUsingDestrObj = function ({name, dob, birthPlace, job, hobbies}) {
  console.log(`Hi ${name}, as per our details you are born in ${birthPlace} and have a job as a ${job}.`);
}

// We can also specify default values in case the properties are not present in the object being passed in
const printPersonDetailsUsingDestrObjAndDefaults = function ({name, dob = '1/1/2020', birthPlace, job, hobbies}) {
  console.log(`Hi ${name}, as per our details you are born in ${birthPlace} and have a job as a ${job}.`);
}
const alice = {
  name: 'Alice',
  dob: '1/1/1990',
  birthPlace: 'New York',
  job: 'Teacher',
  hobbies: ['rock climbing', 'pottery']
}

printPersonDetailsUsingObj(alice);
printPersonDetailsUsingDestrObj(alice);
```

  

  

  

  

**Methodology of Using DataStructures.**

![[Images-attachments/Untitled 22.png|Untitled 22.png]]

![[Images-attachments/Untitled 1 2.png|Untitled 1 2.png]]

  

## Lecture 105: The Spread Operator

- We can use the spread operator to unpack an entire array or objects at once

```JavaScript
// Suppose we have an existing array, and using that we want to create a new array
const myArray1 = [5,6,7,8];
// The new array is supposed to contain all the numbers from 1 to 8
// We can do this. Also, take note that myArray2 is a new array.(Signified by the use of [] operator to create a new array)
const myArray2 = [1,2,3,4, ...myArray1];
console.log(myArray2); // Prints: [1, 2, 3, 4, 5, 6, 7, 8]
// Note that doing:
const myArray3 = [1,2,3,4,myArray1];
// would have given us nested array. That is not what we want.
console.log(myArray2); // Prints: [1, 2, 3, 4, [5, 6, 7, 8]]

// A second use of the spread operator is to pass multiple arguments to a function.
// For example, in console.log, we can pass in the individual elements of the array
console.log(...myArray2); // Prints: 1 2 3 4 5 6 7 8

// Two use cases of the spread operator are:
// 1) Create shallow copies of arrays
// 2) Merge two or more arrays

// 1) Create shallow copies of arrays
const myArrayCopy = [...myArray2];
console.log(myArrayCopy); // Prints: [1, 2, 3, 4, 5, 6, 7, 8]

// 2) Merge two or more arrays
const myArray4 = [9, 10];
const mergedArray = [...myArray2, ...myArray4];
console.log(mergedArray); // Prints: [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

// Note that the spread operator does not only work on arrays, instead it works on all iterables.
// Iterables are: arrays, strings, maps, sets. But NOT Objects.

// Since strings are also iterables, we can also use the spread operator with strings
const firstName = 'Alice';
const charsOfName = [...firstName];
console.log(charsOfName); // Prints: ["A", "l", "i", "c", "e"]

// We can also use the spread operator in order to pass multiple args to a function
const cookFood = function (ing1, ing2, ing3) {
  console.log(`Your meal containing ${ing1}, ${ing2}, and ${ing3} is ready`);
}

const ingredients = ['Potatoes', 'Onions', 'Bread'];
// And now, instead of passing in each of the ingredients like ingredients[0], ingredients[1], ingredients[2], we do this:
cookFood(...ingredients); // Prints: Your meal containing Potatoes, Onions, and Bread is ready

// But now, you can also use the spread operator to perform shallow copy of objects
const bob = {
  name: 'Bob',
  dob: '1/1/1990',
  birthPlace: 'New York',
  job: 'Teacher',
  hobbies: ['rock climbing', 'pottery']
};

// Adding properties to objects using the spread operator
const bobAdd = {
  favFood: 'Ramen',
  ...bob,
  favDrink: 'Kombucha'
};
console.log(bobAdd); // Prints: {favFood: "Ramen", name: "Bob", dob: "1/1/1990", birthPlace: "New York", job: "Teacher, ...}

// Shallow copy of objects
const bobClone = {...bobAdd};
console.log(bobClone); // Prints: {favFood: "Ramen", name: "Bob", dob: "1/1/1990", birthPlace: "New York", job: "Teacher, ...}
```

## Methodology of Using DataStructures.

![[Images-attachments/Untitled 22.png|Untitled 22.png]]

![[Images-attachments/Untitled 1 2.png|Untitled 1 2.png]]

  

## Lecture 106: Rest pattern and Rest Parameters

- The Rest pattern is, in a way, the 'opposite' of the spread operator. It uses the same operator, but to condense multiple values into an array.

```JavaScript
// Just like the Spread operator, the Rest pattern also have two distinct uses.

// 1) Uses it as destructuring

// We have seen the use of the spread operator on the RHS of the = operator
const anotherArray1 = [1,2,...[3,4,5]];
// Now suppose if we wanted to save the first and second elements of the array in variables of their own,
// but for the remaining values, we wanted to group them into one single array.
// This is how we would do it.
// This is the REST syntax, because it is on the left side of the = operator
const [v1, v2, ...arr1] = anotherArray1;
console.log(v1, v2, arr1); // Prints: 1 2 [3,4,5]

// Similarly we can also use the spread operator with objects
const charlie = {
  name: 'Charlie',
  dob: '1/1/1990',
  birthPlace: 'New York',
  job: 'Teacher',
  hobbies: ['rock climbing', 'pottery']
};
// Suppose we wanted to extract the 'hobbies' and store it into a separate variable
const { hobbies, ...otherDetailsAboutCharlie} = charlie;
console.log(hobbies); // Prints: ["rock climbing", "pottery"]
// Note that otherDetailsAboutCharlie no longer contains the hobbies
console.log(otherDetailsAboutCharlie); // Prints: {name: "Charlie", dob: "1/1/1990", birthPlace: "New York", job: "Teacher"}

// 2) Use it with Function arguments

// The Rest operator in this case collects all of the multiple inputs and passes it in as an array.
// So, in effect, this function can accept any number of parameters
const addMultipleNumbers = function (...numbers) {
  console.log(`Args is: ${numbers} of type ${typeof numbers}`);
  let sum = 0;
  for (let i = 0; i< numbers.length; i++) {
    sum += numbers[i];
  }
  console.log(`The final sum is ${sum}`);
}

// Calling the addMultipleNumbers functions with multiple args
addMultipleNumbers(1,2);
addMultipleNumbers(1,2,3,4,);
addMultipleNumbers(1,2,3,4,5);

// Calling the addMultipleNumbers functions with arrays.
// Note how we have an array give, we are destructuring it by using the spread operator, then passing it into the fn
// where the function is then collecting it as a single array and then summing it over.
const arraysAsArgs = [1,2,3,4,5,6];
addMultipleNumbers(...arraysAsArgs);
// This does not work if you do just this.
// I think what happens is that since in the function definition you are using the ... operator
// and you are passing in an array as an args, it is expecting an array of arrays as an input?
addMultipleNumbers(arraysAsArgs);

// Another example using the Rest parameters

// Consider the following function:
const orderFood = function (mainIngredient, ...otherIngredients) {
  console.log(mainIngredient); // Prints: flour
  console.log(otherIngredients); // Prints: ["oil", "butter"]
}

let ingred1 = 'flour';
let ingred2 = 'oil';
let ingred3 = 'butter';
orderFood(ingred1, ingred2, ingred3);
```

## Lecture 107: Short Circuiting

- The `&&` and `||` operators work very differently in JS when compared to Java.Boolean operators can use any datatype as its operands, they can return any datatype, and they do short-circuiting.The OR operator (`||`) will return the _first_ 'truthy' value among all of its operands or the _last_ value if all of them are 'falsy'.The AND operator (`&&`) will return the _first_ 'falsy' value among all of it's operands or return the _last_ value if all of them are 'truthy'.We can use the OR operator to set default values.We can use the AND operator to execute code in the second operand if the first operand is true.

```JavaScript
////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
// Lecture 107: Short Circuiting
////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////

// Normally we have used the && and || operators with only boolean operands
// But we can also use these operators with non-boolean values.
// This shows that the short-circuit operators can accept non-boolean values as arguments
// and at the same time also return non-boolean values.
console.log(3 || 'Alice'); // Prints: 3

// 1) The OR Operator

// In case of the || operator, it will return the first truthy value that it encounters.
// If all the values are falsy, then it will return the last value, even if the last value is false
console.log('' || 'Alice'); // Prints: Alice
console.log(true || 0); // Prints: true
console.log(undefined || null); // Prints: null
console.log(undefined || 0 || '' || 'Hello' || 23); // Prints: Hello. Because 'Hello' is the first truthy value in this chain of operands

// A practical application of this behavior is as follows:
const dave = {
  name: 'Dave',
  dob: '1/1/1990',
  birthPlace: 'New York',
  job: 'Teacher',
  hobbies: ['rock climbing', 'pottery']
};
// Suppose we want to check if a property is present on the object, and we want to use that:
let daveFriends = dave.friends ? dave.friends : [];
console.log(daveFriends); // Prints: [] since there is no property called 'friends' defined on the dave object
// Instead we could have done just this:
// If the property 'friends' does not exist on the dave object, then we assign 'daveFriends' to be [], else the actual value is assigned
daveFriends = dave.friends || [];
console.log(daveFriends); // Prints: []
// But now if we added a property
dave.friends = ['Alice', 'Bob'];
// And tried fetching the value again
daveFriends = dave.friends || [];
console.log(daveFriends); // Prints: ["Alice", "Bob"]

// 2) The AND Operator

// In case of the && operator, it will return the first falsy value that it encounters.
// If all the values are truthy, then it will return the last value, even if the last value is truthy
console.log(0 && 'Alice'); // Prints: 0
console.log('Alice' && 'Bob'); // Prints: 'Bob'
console.log(23 && 'Alice' && true && null && 'Bob'); // Prints: null

// A practical application of this behavior is as follows:
// Suppose we are trying to call a method on the dave object. But we are not sure of the method exists.
// We can wrap the method inside an if-block
if (dave.printFriendList) {
  dave.printFriendList();
}
// But now, with the AND operator we can do this:
// If the printFriendList property is falsy, the method is never called
dave.printFriendList && dave.printFriendList(); // Nothing is printed
// But now if we add the property
dave.printFriendList = function () {
  console.log(this.friends);
};
dave.printFriendList && dave.printFriendList(); // This prints now ["Alice", "Bob"]

////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
// Lecture 108: The Nullish Coalescing Operator
////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////

// Add a new property to the dave object with the falsy value of 0.
dave.numOfFriends = 0;
const numFriends = dave.numOfFriends || -1;
console.log(numFriends); // Prints: -1
// So even though the property 'numOfFriends' is defined, and has a value of 0, the 'numFriends' prints -1.
// This happens because 0 is a falsy value, and like we saw earlier, this will return -1.
// But we want the numFriends to be 0.

// Here we use the new Nullish Coalescing Operator
// This operator works with the concept of Nullish Values, instead of falsy values
// Nullish values are: 'null' and 'undefined'. It does nto include 0 or the empty string
const correctNumOfFriends = dave.numOfFriends ?? -1;
console.log(correctNumOfFriends); // Prints: 0
```

  

## Lecture 110: Looping Arrays - The for-of loop

  

- This is how we can use the new for-of loop in JS.

```JavaScript
const eve = {
  name: 'Eve',
  dob: '1/1/1990',
  birthPlace: 'New York',
  job: 'Teacher',
  hobbies: ['rock climbing', 'pottery'],
  friends: ['Alice', 'Bob', 'Charlie', 'Dave', 'Felicia']
};

// Suppose we want to iterate over the friends property of eve
// We can do it without having to use a counter and incrementing it
// One important point is that in case of the for-of loop, the 'continue' and the 'break' keywords will work
// We will look at some other ways of looping in an array in which they won't. So it is an imp distinction to make
for (const friend of eve.friends) {
  console.log(`${friend}`); // Prints the list of friends
}
// Note that in the above example, we cannot access the index that we are in the array currently
// We can access the current index as well - like this:
for (const friend of eve.friends.entries()) {
  console.log(friend);
}
// Prints:
/*
[0, "Alice"]
[1, "Bob"]
[2, "Charlie"]
and so on... The point being that the index of the element is printed along with the element as an array with two elements
*/

// So you can access the element along with the index using:
// Note here how we are using the destructuring operator
for (const [idx, elm] of eve.friends.entries()) {
  console.log(`Friend ${idx}: ${elm}`);
}
// Prints:
/*
Friend 0: Alice
Friend 1: Bob
Friend 2: Charlie
and so on...
*/
```

## Lecture 111: Enhanced Object Literals

- An object literal is when you create a new object by using the `{}` operator. ES6 introduced 3 new ways in which we can make writing object literals easier.

```JavaScript
// Instead of writing properties explicitly in the object, we can create objects outside the object and then refer to it
let friends = {
  alice: {
    name: 'Alice Doe',
    phone: '111-111-1111'
  },
  bob: {
    name: 'Bob Doe',
    phone: '222-222-2222'
  },
  charlie: {
    name: 'Charlie Doe',
    phone: '333-333-3333'
  },
}

// 1st Addition:
// And now we are referencing the object
const felicia = {
  name: 'Felicia',
  dob: '1/1/1990',
  birthPlace: 'New York',
  job: 'Teacher',
  hobbies: ['rock climbing', 'pottery'],
  friendsList: friends
};

// No need for creating an additional property. Just writing 'friends' will add a new property to the object with the same name as 'friends'
const george = {
  name: 'George',
  dob: '1/1/1990',
  birthPlace: 'New York',
  job: 'Teacher',
  hobbies: ['rock climbing', 'pottery'],
  friends
};


// 2nd Addition:
// We can now write methods in objects in a shorter way
const hannah = {
  name: 'Hannah',
  dob: '1/1/1990',
  birthPlace: 'New York',
  job: 'Teacher',
  hobbies: ['rock climbing', 'pottery'],
  friends,

  // Older way of writing methods
  printFriends: function() {
    for (const friend of this.friends) {
      console.log(friend);
    }
  },

  // Newer, shorter way of writing methods:
  printFriendsNew() {
    for (const friend of this.friends) {
      console.log(friend);
    }
  }
};

// 3rd Addition
// Instead of just writing the property names, we can now even compute the property names
const days = ['mon', 'tue', 'wed', 'thu', 'fri', 'sat', 'sun'];
const ila = {
  workingHours: {
    // Note how we are specifying the property names in []. We discussed this way back.
    [days[0]]: {
      leave: '8:00am',
      arrive: '3:00pm'
    },
    [days[1]]: {
      leave: '8:00am',
      arrive: '4:00pm'
    },
    // We can also compute the property names
    [`${'w' + 'e' + 'd'}`] : {
      leave: '8:00am',
      arrive: '5:00pm'
    }
  }
}

console.log(ila.workingHours.mon); // Prints: {leave: "8:00am", arrive: "3:00pm"}
console.log(ila.workingHours.tue); // Prints: {leave: "8:00am", arrive: "4:00pm"}
console.log(ila.workingHours.wed); // Prints: {leave: "8:00am", arrive: "5:00pm"}
```

  

## Lecture 112: Optional Chaining

- The Optional Chaining operator  `?.`  is used to check if variable _immediately_ preceding the operator exists or not. By 'exists' we mean the nullish concept and not the truthy/false concept. A value is nullish only if it is either `null` or `undefined`. If the variable preceding the operator is nullish, then the execution of the statement will immediately return with the value of `undefined`. Else, the execution will proceed as normal.

```JavaScript
const jake = {
  workingHours: {
    mon: {
      leave: '8:00am',
      arrive: '3:00pm'
    },
    tue: {
      leave: '8:00am',
      arrive: '4:00pm'
    },
    wed : {
      leave: '8:00am',
      arrive: '5:00pm'
    }
  },

  printWorkingHoursFor(day) {
    // console.log(`${this.workingHours}`)
    return `Leave at ${this.workingHours[day].leave} and arrive at ${this.workingHours[day].arrive}`;
  }
};

// Suppose we were trying to find the working hours of jake on friday. There is no property called 'fri' on the jake object
console.log(jake.workingHours.fri); // Prints: undefined
// But suppose we did not know that, and tried to access the 'leave' property on 'fri'
// console.log(jake.workingHours.fri.leave); // Uncaught TypeError: Cannot read property 'leave' of undefined
// So we would like to have a null check for the 'jake.workingHours.fri' property before we access any properties on it

// We could do that by enclosing it in an if-block or in the && operator
if (jake.workingHours.fri) {
  console.log(jake.workingHours.fri.leave);
}
// or:
jake.workingHours.fri && console.log(jake.workingHours.fri.leave);

// But in ES6, we can use the optional chaining operator.
// Only if the property specified just before the ? exists, only then will the next property will be accessed.
// If not, then immediately 'undefined' will be returned.
// Here 'exists' means the 'Nullish' concept that we talked about before. So the 'leave' property will be accessed
// only if the 'fri' property is either null or undefined. Not if it is empty, or 0, or any other such falsy values
console.log(jake.workingHours.fri?.leave); // undefined

// Consider the following example
const daysOfWeek = ['mon', 'tue', 'wed', 'thu', 'fri', 'sat', 'sun'];
for (const day of daysOfWeek) {
  // Note how we are dynamically using the 'day' variable to specify the property name on the object.
  // If we want to use a variable name as the property name, then we have to use the [] notation.
  // and then using the optional chaining operator to access the 'leave' property on the object only if the object
  // jake.workingHours.day is not null
  console.log(`Work starts at ${jake.workingHours[day]?.leave}`);
}
// Prints:
/*
Work starts at 8:00am
Work starts at 8:00am
Work starts at 8:00am
Work starts at undefined
Work starts at undefined
Work starts at undefined
Work starts at undefined
 */

// 2)
// Optional Chaining also works on methods - we can check if a method actually exists before we call it

// In this line we are first checking if the printWorkingHoursFor method is defined on the jake object.
// If it is not defined, then we print 'Method does not exist' using the Null Coalescing Operator
console.log(jake.printWorkingHoursFor?.('tue') ?? 'Method does not exist'); // Prints: 'Leave at 8:00am and arrive at 4:00pm'
console.log(jake.printWorkingHours?.('tue') ?? 'Method does not exist'); // Prints: 'Method does not exist'


// 3)
// Optional Chaining also works on Arrays
const users = [{name: 'Alice', email:'alice@e.com'}];
// Remember that the ?. operator tests if the object immediately preceding the operator exists or not
// And by exist we again mean the nullish concept, and not the falsy-truthy concept
// So here, it will check if the users[0] exists
console.log(users[0]?.name ?? 'Array is empty'); // Prints: 'Alice'
console.log(users[1]?.name ?? 'Array is empty'); // Prints: 'Array is empty'
```

## Lecture 113: Looping Objects - Object Keys, Values, and Entries

- We can use the for-of operator to loop through either the keys, values, or both keys and values of an object.

```JavaScript
// Previously we used the for-of loop to loop over arrays, and we said that only iterables can be looped over using the for-of loop
// But we can also use the for-of loop to loop over objects

const kristin = {
  workingHours: {
    mon: {
      leave: '8:00am',
      arrive: '3:00pm'
    },
    tue: {
      leave: '8:00am',
      arrive: '4:00pm'
    },
    wed : {
      leave: '8:00am',
      arrive: '5:00pm'
    },
    thu : {
      leave: '8:00am',
      arrive: '5:00pm'
    }
  },

  printWorkingHoursFor(day) {
    // console.log(`${this.workingHours}`)
    return `Leave at ${this.workingHours[day].leave} and arrive at ${this.workingHours[day].arrive}`;
  }
};

// There are three use cases - we can either loop over the keys (property names), or the values in the object, or both keys and values together

// 1) Loop over the keys (property names)
for (const day of Object.keys(kristin.workingHours)) {
  console.log(day);
}
/*
  Prints:
  mon
  tue
  wed
  thu
 */

// So what is exactly going on?
// As you can see, Object.keys prints an array containing the keys of the object. Then we are just looping over the array
// It only creates an array from the top level properties, does not pick up nested properties of the object passed in as an arg
const keysInTheObject = Object.keys(kristin.workingHours);
console.log(keysInTheObject); // Prints: ["mon", "tue", "wed", "thu"]

// 2) Loop over the values

const valuesInTheObject = Object.values(kristin.workingHours);
console.log(valuesInTheObject);
// We get an array of 4 objects
/*
0: {leave: "8:00am", arrive: "3:00pm"}
1: {leave: "8:00am", arrive: "4:00pm"}
2: {leave: "8:00am", arrive: "5:00pm"}
3: {leave: "8:00am", arrive: "5:00pm"}
*/

// Similar to the previous case, we can log the values now
for (const {leave, arrive} of Object.values(kristin.workingHours)) {
  console.log(`Kristin leaves at ${leave} and arrives at ${arrive}`);
}
/*
Prints:
Kristin leaves at 8:00am and arrives at 3:00pm
Kristin leaves at 8:00am and arrives at 4:00pm
Kristin leaves at 8:00am and arrives at 5:00pm
Kristin leaves at 8:00am and arrives at 5:00pm
*/

// 3) Loop over the both property names and the values - so basically entries
const entriesInTheObject = Object.entries(kristin.workingHours);
console.log(entriesInTheObject);
/*
Note that this is an array
[Array(2), Array(2), Array(2), Array(2)]
[["mon", {…}], ["tue", {…}], ["wed", {…}], ["thu", {…}]]
Prints:
  0: (2) ["mon", {…}]
  1: (2) ["tue", {…}]
  2: (2) ["wed", {…}]
  3: (2) ["thu", {…}]
 */

// And we can loop over the entries in the object like this:
// Note the way that we are destructuring the object. The outermost is an array notation [], and not object notation {}
for (const [day, {leave, arrive}] of Object.entries(kristin.workingHours)) {
  console.log(`On ${day}, Kristin leaves at ${leave} and arrives at ${arrive}`);
}
/*
Prints:
On mon, Kristin leaves at 8:00am and arrives at 3:00pm
On tue, Kristin leaves at 8:00am and arrives at 4:00pm
On wed, Kristin leaves at 8:00am and arrives at 5:00pm
On thu, Kristin leaves at 8:00am and arrives at 5:00pm
 */
```

## Lecture 115: Sets

- In ES6, two new datastructures were added to JS - Sets and Maps.A set is a collection of unique values - there can be no duplicate elements in a set. Hence, sets are often used to remove duplicate elements from an array.A set can hold mixed datatypes.In the args for the constructor of the set, we can pass in any kind of iterable. Since an array is an iterable, it works. Similarly, if you pass in a single string, that would work as well. The resulting set that would be created would contain the unique characters that are present in the string.

```JavaScript
const foodItemsArr = ['Pizza', 'Pasta', 'Pork', 'Pasta', 'Plums'];
const foodItemsSet = new Set(foodItemsArr);
console.log(foodItemsSet); // Prints: {"Pizza", "Pasta", "Pork", "Plums"}

// Just like arrays, sets are also iterables
// Remember that strings are also iterables, hence we can also create a set of characters by using a string
const charsInPizza = new Set('Pizza');
console.log(charsInPizza); // Prints: {"P", "i", "z", "a"}

// Prints the size of the array
console.log(charsInPizza.size); // Prints: 4

// Check if an element exists in a set
console.log(charsInPizza.has('P')); // Prints: true
console.log(charsInPizza.has('Q')); // Prints: false

// We can add elements to a set
foodItemsSet.add('Peas');
console.log(foodItemsSet); // Prints: {"Pizza", "Pasta", "Pork", "Plums", "Peas"}

// We can remove elements from a set
foodItemsSet.delete('Plums');
console.log(foodItemsSet); // Prints: {"Pizza", "Pasta", "Pork", "Peas"}

// Delete all the elements from a set
charsInPizza.clear();
console.log(charsInPizza); // Prints: {}

// Since sets are iterable, we can iterate over a set as well
for (const item of foodItemsSet) {
  console.log(item);
}
/*
  Prints:
  Pizza
  Pasta
  Pork
  Peas
*/

// Sets can be used to remove duplicate elements from an array
const groceryList = ['Pizza', 'Pasta', 'Pork', 'Pasta', 'Plums'];
// Now we want to remove the duplicate items from the groceryList
// In the args for the constructur of the set, we pass in any kind of iterable. Since an array is an iterable, it works.
const uniqueItemsInList = new Set(groceryList);
// And now we can store it back into an array.
// Since the spread operator can take in any iterable as an arg, we can simply do this
const groceryListUnique = [...uniqueItemsInList];
console.log(groceryListUnique); // Prints: ["Pizza", "Pasta", "Pork", "Plums"]

// You could just do the entire thing in one line
const uniqueItemsAgain = [...new Set(groceryList)];
console.log(uniqueItemsAgain); // Prints: ["Pizza", "Pasta", "Pork", "Plums"]
```

## Lecture 116: Maps

- Just like an object, the data in a map is stored as key-value pairs. The difference between the two is that in the case of maps, the keys can have _any_ type, whereas in the case of objects, the keys _have to_ be strings. So maps can have an object, an array, or even another map as a key.Note that the insertion order is maintained in maps in JS as mentioned [here on SO](https://stackoverflow.com/a/50862944/8742428).

```JavaScript
const hannah2 = {
    name: 'Hannah',
    dob: '1/1/1990',
    birthPlace: 'New York',
    job: 'Teacher',
    hobbies: ['rock climbing', 'pottery'],
    friends,

    // Older way of writing methods
    printFriends: function () {
        for (const friend of this.friends) {
            console.log(friend);
        }
    },

    // Newer, shorter way of writing methods:
    printFriendsNew() {
        for (const friend of this.friends) {
            console.log(friend);
        }
    }
};
// Create a new map
const laura = new Map();

// To add elements to a map, we use the set method
laura.set('name', 'Laura Doe');
// We can use different datatypes for keys and values
laura.set(1, 'Alice');
laura.set(2, 'Bob');

// Calling the set() method returns the new map. Hence we can chain the set() operations
laura.set('hobbies', ['rock climbing', 'pottery']).set(3, 'Charlie');

// We can also use boolean values as the key
laura.set(true, 'Laura is available');
laura.set(false, 'Laura is Busy');

// If we log the map now, we see the details
console.log(laura);

/*
Prints:
    Map(7){"name" => "Laura Doe", 1 => "Alice", 2 => "Bob", "hobbies" => Array(2), "3" => "Charlie",…}
    [[Entries]]
    0: {"name" => "Laura Doe"}
    1: {1 => "Alice"}
    2: {2 => "Bob"}
    3: {"hobbies" => Array(2)}
    4: {3 => "Charlie"}
    5: {true => "Laura is available"}
    6: {false => "Laura is Busy"}
 */


// To retrieve elements from the map, we use the get() method
console.log(laura.get('name')); // Prints: 'Laura Doe'
console.log(laura.get(true)); // Prints: 'Laura is available'
// Note that the datatype of the key matters
// For eg. this will print 'undefined' because there is no key with string 'true'
console.log(laura.get('true')); // Prints: 'undefined'

// We can also check whether a map has a certain key
console.log(laura.has('hobbies')); // Prints: true
console.log(laura.has('friends')); // Prints: false

// We can also delete elements from the mapp by passing in the respective key as the argument
laura.delete(3);
console.log(laura);
/*
    Prints:
    0: {"name" => "Laura Doe"}
    1: {1 => "Alice"}
    2: {2 => "Bob"}
    3: {"hobbies" => Array(2)}
    4: {true => "Laura is available"}
    5: {false => "Laura is Busy"}
 */

// We cal also use objects/arrays as map keys

// First let us look at the wrong way of doing it:
// We are adding a new element
laura.set([0, 1], 'An Array');
// And now we try to get the same element
console.log(laura.get([0, 1])); // Prints: 'undefined'
// Why? Because recall that objects are stored by their reference.
// SO you need to store the reference once you have created an array.

// So the correct way of doing it is:
const arrayKey = [1,2];
laura.set(arrayKey, 'Another Array');
console.log(laura.get(arrayKey)); // Prints: 'Another Array'

// Just to visually confirm that the array is in fact being stored as the key
console.log(laura);
/*
0: {"name" => "Laura Doe"}
1: {1 => "Alice"}
2: {2 => "Bob"}
3: {"hobbies" => Array(2)}
4: {true => "Laura is available"}
5: {false => "Laura is Busy"}
6: {Array(2) => "An Array"}
    key: (2) [0, 1]
    value: "An Array"
7: {Array(2) => "Another Array"}
    key: (2) [1, 2]
    value: "Another Array"
 */


// You can get the size of a map by doing
console.log(laura.size); // Prints: 6

// We can remove all the elements from the map
laura.clear();
console.log(laura); // Prints: Map(0) {}
```

## Lecture 117: Maps - Iteration

- Instead of using the `set` method to add elements to a map, you can just add the elements all at once by passing in an array as shown in the example below. This allows us to easily create maps from existing objects.Similarly, we can use existing maps to convert them into arrays by using the spread operator.
- Instead of using the `set` method to add elements to a map, you can just add the elements all at once by passing in an array as shown in the example below. This allows us to easily create maps from existing objects.Similarly, we can use existing maps to convert them into arrays by using the spread operator.

```JavaScript
// There are other ways of populating a new map other than using the set() method that we used previously
const maria = new Map([
    ['name', 'Maria Doe'],
    ['friends', ['Alice', 'Bob', 'Charlie']],
    ['hobbies', ['Stamp Collecting']]
]);

console.log(maria); // Prints: {"name" => "Maria Doe", "friends" => Array(3), "hobbies" => Array(1)}

// Note that the constructor for the Map in the above example takes an array of arrays as an arg.

const kristin2 = {
    workingHours: {
        mon: {
            leave: '8:00am',
            arrive: '3:00pm'
        },
        tue: {
            leave: '8:00am',
            arrive: '4:00pm'
        },
        wed: {
            leave: '8:00am',
            arrive: '5:00pm'
        },
        thu: {
            leave: '8:00am',
            arrive: '5:00pm'
        }
    }
};

// Recall the Object.entries() method that we looked at back in Lecture 113
const kristinActiveHours = Object.entries(kristin2.workingHours);
console.log(kristinActiveHours);
/*
Prints:
0: (2) ["mon", {…}]
1: (2) ["tue", {…}]
2: (2) ["wed", {…}]
3: (2) ["thu", {…}]
 */

// Converting Objects to a Map

// So this gives us an efficient way of converting objects to map
const kristinHoursMap = new Map(kristinActiveHours);
console.log(kristinHoursMap);
/*
Prints:
0: {"mon" => Object}
key: "mon"
value: {leave: "8:00am", arrive: "3:00pm"}
1: {"tue" => Object}
key: "tue"
value: {leave: "8:00am", arrive: "4:00pm"}
2: {"wed" => Object}
key: "wed"
value: {leave: "8:00am", arrive: "5:00pm"}
3: {"thu" => Object}
key: "thu"
value: {leave: "8:00am", arrive: "5:00pm"}
 */

// We can iterate over the map by doing
for (const [day, {leave, arrive}] of kristinHoursMap) {
    console.log(`On ${day}, Kristin2 leaves at ${leave} and arrives at ${arrive}`);
}

/*
Prints:
    On mon, Kristin2 leaves at 8:00am and arrives at 3:00pm
    On tue, Kristin2 leaves at 8:00am and arrives at 4:00pm
    On wed, Kristin2 leaves at 8:00am and arrives at 5:00pm
    On thu, Kristin2 leaves at 8:00am and arrives at 5:00pm
 */


// Converting Map back to Array
// By just using the spread operator and unpacking the map into an array -
console.log([...kristinHoursMap]);
/*
0: (2) ["mon", {…}]
1: (2) ["tue", {…}]
2: (2) ["wed", {…}]
3: (2) ["thu", {…}]
 */
```

## Lecture 118: Working with Strings

- Here are some examples of working with strings

```JavaScript
////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
// Lecture 120: Working with Strings Part 1
////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////

const airline = 'TAP Air Portugal';
const plane = 'A320';

console.log(plane[0]); // Prints: 'A'
console.log(plane[1]); // Prints: '3'
console.log(plane[2]); // Prints: '2'
console.log('B737'[0]); // Prints: 'B'

console.log(airline.length); // Prints: 16
console.log('B737'.length); // Prints: 4

console.log(airline.indexOf('r')); // Prints: 6

// Search from the end of the string
console.log(airline.lastIndexOf('r')); // Prints: 10

// Search for the occurrence of the word
console.log(airline.indexOf('portugal')); // Prints: -1. Because it is case-sensitive and string contains 'Portugal' not 'portugal'

// Slice the string starting from index 4 to the end of the string
console.log(airline.slice(4)); // Prints: 'Air Portugal'

// Slice the string from index 4 included, index 7 excluded
console.log(airline.slice(4, 7)); // Prints: 'Air'

// Start from the index 0, right up to the first occurrence of the ' '
console.log(airline.slice(0, airline.indexOf(' '))); // Prints: 'TAP'

// Start from the index AFTER the last index of ' ', right up to the end of the string
console.log(airline.slice(airline.lastIndexOf(' ') + 1)); // Prints: 'Portugal'

// Start from the end of the string. Print the first two characters
console.log(airline.slice(-2)); // Prints: 'al'

// Start from the index 1 print up to the index before the last one
console.log(airline.slice(1, -1)); // Prints: 'AP Air Portuga'

// JS internally changes the type of string by boxing and unboxing as required
console.log(typeof 'JavaScript Strings'); // Prints: string
console.log(typeof new String('JavaScript Strings')); // Prints: object. Warning: Primitive type object wrapper used
console.log(typeof String('JavaScript Strings')); // Prints: string

////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
// Lecture 121: Working with Strings Part 2
////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////

const anotherAirline = 'TAP Air Portugal';

console.log(anotherAirline.toLowerCase());
console.log(anotherAirline.toUpperCase());

// Comparing emails
const email = 'hello@jonas.io';
const loginEmail = '  Hello@Jonas.Io \n';
// Note that the trim() method will also remove the \n character from the email
const normalizedEmail = loginEmail.toLowerCase().trim();
console.log(normalizedEmail);
console.log(email === normalizedEmail); // Prints: true

// replacing
const priceGB = '288,97£';
// replace() calls can also be chained because replace() returns the resulting string
const priceUS = priceGB.replace('£', '$').replace(',', '.');
console.log(priceUS); // Prints: 288.97$

const announcement = 'All passengers come to boarding door 23. Boarding door 23!';

// Note that the replace() method will replace ONLY the first occurrence of the string
// Here 'door' is the value to be searched, and 'gate' is the value to be replaced
console.log(announcement.replace('door', 'gate'));
// Prints: All passengers come to boarding gate 23. Boarding door 23!

// replaceAll() will replace ALL occurrences of the string
console.log(announcement.replaceAll('door', 'gate'));
// Prints: All passengers come to boarding gate 23. Boarding gate 23!

// We can also use regex to replace the string 'door'
// The string to be searched is placed between / / and g is appended to tell JS to replace everywhere
console.log(announcement.replace(/door/g, 'gate'));

// Booleans
const anotherPlane = 'Airbus A320neo';
console.log(anotherPlane.includes('A320')); // Prints: true
console.log(anotherPlane.includes('Boeing')); // Prints: false
console.log(anotherPlane.startsWith('Airb')); // Prints: true

////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
// Lecture 122: Working with Strings Part 3
////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////

// Split and join
console.log('a+very+nice+string'.split('+')); // Prints: ["a", "very", "nice", "string"]
console.log('Alice Doe'.split(' ')); // Prints: ["Alice", "Doe"]

// We can split and store the results directly using destructuring
const [fName, lName] = 'Alice Doe'.split(' ');

// We can join strings:
const newName = ['Ms.', fName, lName].join(' ');
console.log(newName); // Prints: Ms. Alice Doe

// Changing the joiner
const newName2 = ['Ms.', fName, lName].join('_');
console.log(newName2); // Prints: Ms._Alice_Doe

// Capitalize just the first letter of every word of a name
const capitalizeName = function (name) {
    const splitName = name.toLowerCase().split(' ');
    const capitalizedNameArr = [];
    for (const partialName of splitName) {
        capitalizedNameArr.push(partialName[0].toUpperCase() + partialName.slice(1));
    }
    return capitalizedNameArr.join(' ');
}
console.log(capitalizeName('jessica ann smith davis')); // Prints: Jessica Ann Smith Davis

// Padding
// Adding characters to the beginning or the end of the string until the string is of a desired length
const message = 'Go to gate 23!';
console.log(message.padStart(20, '+')); // Prints: ++++++Go to gate 23!
console.log(message.padEnd(20, '+')); // Prints: Go to gate 23!++++++

// First pad the string on the left to 20, then pad the resultant padded string on the right till 30
console.log(message.padStart(20, '+').padEnd(30, '-')); // Prints: ++++++Go to gate 23!---------

// Repeat a string 
const message2 = 'Bad weather...';
console.log(message2.repeat(5));
// Prints: Bad weather...Bad weather...Bad weather...Bad weather...Bad weather...
```