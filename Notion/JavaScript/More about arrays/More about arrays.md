![[Images-attachments/Untitled 24.png|Untitled 24.png]]

## Lecture 141: Simple Array Methods

- Arrays are objects, and as we have seen, objects can have functions of their own that are called methods. There are several methods that are defined on the array object.

1. **Slice**:→ Extract part of the array WITHOUT changing the original array (i.e. returns a new array)

The `slice()` function is a built-in method in JavaScript that allows you to create a new array by extracting a portion of an existing array. It doesn't modify the original array; instead, it returns a shallow copy of a portion of the array as a new array object.

The syntax for the `slice()` function is as follows:

  

```Plain
array.slice(startIndex, endIndex)
```

- `startIndex` is the index at which the extraction should begin. It represents the position in the array where the extraction will start. If `startIndex` is negative, it indicates an offset from the end of the array.
- `endIndex` (optional) is the index at which the extraction should end. The `slice()` function extracts up to, but not including, the element at the `endIndex` position. If `endIndex` is negative, it indicates an offset from the end of the array. If `endIndex` is not provided, the extraction will continue until the end of the array.

The `slice()` function returns a new array containing the extracted elements. The original array remains unchanged.

Here are a few examples to illustrate how `slice()` works:

1. Extracting a portion of an array:

```JavaScript
const fruits = ['apple', 'banana', 'cherry', 'date', 'elderberry'];
const extracted = fruits.slice(1, 4); // Extracts elements from index 1 to index 3
console.log(extracted); // Output: ['banana', 'cherry', 'date']
```

1. Extracting from a specific index to the end of the array:

```JavaScript
const fruits = ['apple', 'banana', 'cherry', 'date', 'elderberry'];
const extracted = fruits.slice(2); // Extracts elements from index 2 to the end
console.log(extracted); // Output: ['cherry', 'date', 'elderberry']
```

1. Creating a shallow copy of an array:

```JavaScript
const original = ['apple', 'banana', 'cherry'];
const copy = original.slice(); // Creates a shallow copy of the original array
console.log(copy); // Output: ['apple', 'banana', 'cherry']
console.log(original === copy); // Output: false (they are separate array objects)
```

In summary, the `slice()` function in JavaScript allows you to extract a portion of an array to create a new array. It provides a convenient way to work with a subset of array elements without modifying the original array.

1. **Splice**:→MUTATES THE ORIGINAL ARRAY. Functionally, it is same as slice(). but return 1 more index than slice if doubt arises always do it in compiler and confirm.

The `splice()` function is a built-in method in JavaScript that allows you to modify an array by adding or removing elements at a specified index. It provides a flexible way to manipulate arrays and can be used for various purposes, such as inserting, deleting, or replacing elements.

The syntax for the `splice()` function is as follows:

```Plain
array.splice(startIndex, deleteCount, item1, item2, ..., itemX)
```

- `startIndex` is the index at which the modification should begin. It represents the position in the array where the operation will start.
- `deleteCount` is the number of elements to be removed from the array. If set to 0, no elements will be removed.
- `item1, item2, ..., itemX` (optional) are the elements to be added to the array at the specified `startIndex`. These elements are inserted into the array starting from the `startIndex`.

The `splice()` function returns an array containing the removed elements, if any.

Here are a few examples to illustrate how `splice()` works:

1. Removing elements from an array:

A common practice is removing the last element with splice

`arr.splice(-1);`

```JavaScript
const fruits = ['apple', 'banana', 'cherry', 'date'];
const removedElements = fruits.splice(1, 2); // Removes 2 elements starting from index 1
console.log(fruits); // Output: ['apple', 'date']
console.log(removedElements); // Output: ['banana', 'cherry']
```

1. Adding elements to an array:

```JavaScript
const fruits = ['apple', 'date'];
fruits.splice(1, 0, 'banana', 'cherry'); // Inserts 'banana' and 'cherry' at index 1
console.log(fruits); // Output: ['apple', 'banana', 'cherry', 'date']
```

1. Replacing elements in an array:

```JavaScript
const fruits = ['apple', 'banana', 'cherry', 'date'];
fruits.splice(2, 1, 'grape'); // Replaces 1 element at index 2 with 'grape'
console.log(fruits); // Output: ['apple', 'banana', 'grape', 'date']
```

In summary, JavaScript’s splice() function allows you to modify arrays by removing and/or adding elements at a specific index, providing a powerful way to manipulate array contents.

![[Images-attachments/Untitled 1 4.png|Untitled 1 4.png]]

1. **Reverse**: MUTATES THE ORIGINAL ARRAY. Reverses the array
2. **CONCAT**: Join two arrays. Does not change either of the two inout arrays.
3. **JOIN**: join all the elements of an array using the specified separator
4. **push**: Add elements to the end of the array.
5. **pop**: Removes the last element of the array. Returns the removed element.
6. **shift**: Remove the first element of the array.
7. **unshift**: Add elements to the beginning of the array.
8. **indexof**: Find the index at which a particular element is at
9. **includes**: Find if the element exists in an array
10. `arr.at()` for acessing elements has its own advantages over `arr[]`

```JavaScript
// Simple array methods
let arr = ['a', 'b', 'c', 'd', 'e'];

// SLICE: Extract part of the array WITHOUT changing the original array (i.e. returns a new array)
console.log(arr.slice(2)); // Prints: ["c", "d", "e"]
console.log(arr.slice(2, 4)); // Prints: ["c", "d"]
console.log(arr.slice(-2)); // Prints: ["d", "e"]
console.log(arr.slice(-1)); // Prints: ["e"]
console.log(arr.slice(1, -2)); // Prints: ["b", "c"]
console.log(arr.slice()); // Prints: ["a", "b", "c", "d", "e"] i.e. returns a shallow copy of the original array
console.log([...arr]); // But this method of creating a shallow copy is much easier

console.log(arr); // Prints: ["a", "b", "c", "d", "e"]. i.e. original array is not mutated in any way


// SPLICE: MUTATES THE ORIGINAL ARRAY. Functionally, it is same as slice().
console.log(arr.splice(2)); // Prints: ["c", "d", "e"]
// But now, notice what happens to our original array. The above extracted elements are basically gone from the original array
console.log(arr); // Prints: ["a", "b"]
// Hence the splice() method is normally used for deleting one or more elements from the array
// One common use case is to remove the last element from an array
console.log(arr.splice(-1)); // Prints: ["b"]
console.log(arr); // Prints: ["a"]

// Note that the second args of the splice() method is the number of elements that should be deleted
arr = ['a', 'b', 'c', 'd', 'e'];
console.log(arr.splice(2, 2)); // Prints: ["c", "d"]
console.log(arr); // Prints: ["a", "b", "e"]


// REVERSE: MUTATES THE ORIGINAL ARRAY. Reverses the array
arr = ['a', 'b', 'c', 'd', 'e'];
const arr2 = ['j', 'i', 'h', 'g', 'f'];
console.log(arr2.reverse()); // Prints: ["f", "g", "h", "i", "j"]
console.log(arr2); // Prints: ["f", "g", "h", "i", "j"]. The original array is mutated


// CONCAT: Join two arrays. Does not change either of the two inout arrays.
const letters = arr.concat(arr2);
console.log(letters); // Prints: ["a", "b", "c", "d", "e", "f", "g", "h", "i", "j"]
// But we can also concat two arrays using this:
console.log([...arr, ...arr2]);


// JOIN: join all the elements of an array using the specified separator
console.log(letters.join(' - ')); // Prints: a - b - c - d - e - f - g - h - i - j
console.log(typeof letters.join(' - ')); // Note that the type of the result after join is a string

// Just for the sake of completion - we ahve already looked at these methods earlier

// push: Add elements to the end of the array.
arr = ['a', 'b', 'c', 'd', 'e'];
console.log(arr.push('f')); // Prints: 6. Because it returns the size of the resulting array
console.log(arr); // Prints: ["a", "b", "c", "d", "e", "f"]

// unshift: Add elements to the beginning of the array.
arr = ['b', 'c', 'd', 'e'];
console.log(arr.unshift('a')); // Prints: 5. Because it returns the size of the resulting array
console.log(arr); // Prints: ["a", "b", "c", "d", "e", "f"]

// pop: Removes the last element of the array. Returns the removed element.
arr = ['a', 'b', 'c', 'd', 'e'];
console.log(arr.pop()); // Prints: 'e'
console.log(arr); // Prints: ["a", "b", "c", "d"]

// shift: Remove the first element of the array.
arr = ['a', 'b', 'c', 'd', 'e'];
console.log(arr.shift()); // Prints: 'a'
console.log(arr); // Prints: ["b", "c", "d", "e"]

// indexof: Find the index at which a particular element is at
arr = ['a', 'b', 'c', 'd', 'e'];
console.log(arr.indexOf('c')); // Prints: 2
console.log(arr.indexOf('f')); // Prints: -1

// includes: Find if the element exists in an array
arr = ['a', 'b', 'c', 'd', 'e'];
console.log(arr.includes('c')); // Prints: true
console.log(arr.includes('f')); // Prints: false
```

  

## Lecture 142: Looping Arrays using forEach

- We can loop over arrays using either the `for-of` loop that we looked at earlier. Or we can use the `forEach` loop.The difference between the two is that you can use keywords like `break` and `continue` to break out of a `for-of` loop, but you cannot do that with a `forEach` loop. The `forEach` loop will be executed for each element in the collection by default.
- We cannot use `break;` for `forEach`
- `forEach` changes the existing array and does not create the new array

We can use forEach with indexes and it is super easy to do it though

forEach always take the first element as the element of the array

second element as the index and the third as the array itself

**Continue and break statements do not work in forEach!**

```JavaScript
movements.forEach(function (movement, idx, array) {
if (movement > 0) {
console.log(`Movement ${idx + 1}: Gaining ${movement} $`);
}
 else {
console.log(`Movement ${idx + 1}: Withdraw ${movement} $`);
}});
```

```JavaScript
let friends = ['Alice', 'Bob', 'Charlie', 'Dave', 'Eve'];

// We saw earlier how to loop over an iterable using a for-of loop
for (const friend of friends) {
    console.log(`Friend's name: ${friend}`);
}
// And we could access the index in a for-of loop using:
for (const [i, friend] of friends.entries()) {
    if (friend === 'Alice') continue;
    console.log(`${i}: Friend's name: ${friend}`);
}

// Instead, we can use a forEach loop, that uses a callback function.
// forEach, in this case, is an example of a higher-order function because it accepts another function as an arg
// The callback fn in the forEach method accepts 3 parameters:
// 1) the element of the array itself
// 2) the index of the element
// 3) the entire original array
// Just like in previous method, we can pass in only part of the parameters that are required, and that would work as well
friends.forEach(function (friend, index, arr) {
    // if (friend === 'Alice') break; Cannot use 'break' or 'continue' within a forEach loop
    console.log(`${index} - Friend's name in forEach: ${friend}`);
})

/*
0 - Friend's name in forEach: Alice
1 - Friend's name in forEach: Bob
2 - Friend's name in forEach: Charlie
3 - Friend's name in forEach: Dave
4 - Friend's name in forEach: Eve
*/
```

## Lecture 143: forEach with Maps and Sets

- Similar to arrays, `forEach` can also work with maps and sets.

```JavaScript
// forEach With Maps and Sets

// Map
const currencies = new Map([
    ['USD', 'United States dollar'],
    ['EUR', 'Euro'],
    ['GBP', 'Pound sterling'],
]);

currencies.forEach(function (value, key, currencyMap){
    console.log(`Symbol: ${key}, stands for ${value}`);
});

/*
Prints:
Symbol: USD, stands for United States dollar
Symbol: EUR, stands for Euro
Symbol: GBP, stands for Pound sterling
 */

// Sets
const currenciesUnique = new Set(['USD', 'GBP', 'USD', 'EUR', 'EUR']);
console.log(currenciesUnique);
// Since there are no keys in a set, the 'value' args is passed in as the 'key' arg as well
currenciesUnique.forEach(function (value, key, map) {
    console.log(`${key}: ${value}`);
});
/*
Prints:
USD: USD
GBP: GBP
EUR: EUR
 */

// In JS we have a convention, we can use '_' to denote a throwaway variable. Hence the above can be rewritten as:
currenciesUnique.forEach(function (value, _, map) {
    console.log(`${value}`);
});
```

## Lecture 145: Creating DOM Elements

- There are multiple ways in which we can create and add elements to the DOM.
- Using `insertAdjacentHTML` method is one such way. We will look at other ways later in the course.

```JavaScript
// We can dynamically generate HTML elements
const alice = {
    name: 'Alice Doe',
    age: 21,
    birthYear: 1990,
    friends: ['Bob', 'Charlie', 'Dave', 'Eve'],
}

// First, select the div that you want to insert the html into
const movementContainer = document.querySelector('.movements');

const displayFriendsFor = function (person) {

    // This is an important method
    // This returns the entire inner html inside the movementContainer
    console.log(movementContainer.innerHTML);

    /*
    Prints:
    <div class="movements__row">
          <div class="movements__type movements__type--deposit">2 deposit</div>
          <div class="movements__date">3 days ago</div>
          <div class="movements__value">4 000€</div>
        </div>
        <div class="movements__row">
          <div class="movements__type movements__type--withdrawal">
            1 withdrawal
          </div>
          <div class="movements__date">24/01/2037</div>
          <div class="movements__value">-378€</div>
    </div>
     */

    // We looked at a similar property called 'textContent' earlier. That returned only the text, innerHTML returns the entire HTML
    console.log(movementContainer.textContent);
    /*
    Prints:
    2 deposit
          3 days ago
          4 000€
            1 withdrawal
          24/01/2037
          -378€
     */

    // Here, we are using it as a setter, removing the existing HTML.
    movementContainer.innerHTML = '';
    console.log(movementContainer.innerHTML); // Prints the empty string

    // Now we loop over each of the friends, adding a new div containing the friend name into selected element
    person.friends.forEach(
        function (friend, idx) {
            const html = `<div class="movements__row">Friend ${idx + 1}: ${friend}</div>`;
            // We use the insertAdjacentHTML to add HTML
            // 'afterbegin' denotes the position where you want to insert HTML.
            // There are 4 different places where html can be inserted, check MDN
            movementContainer.insertAdjacentHTML("afterbegin", html);
        })
}

displayFriendsFor(alice);
```

## Lecture 147: Data Transformations - map, filter, reduce

- This image shows what each of these methods do on a high level:

![[Images-attachments/Untitled 2 3.png|Untitled 2 3.png]]

## Lecture 148: The map method

- The `map` method returns a new array. It does not mutate the original array.

- For every element in the original array, the callback function passed into the map method is executed on that element. The result is stored in a new array, and that array is returned as the result.

A rule of thumb is that the `forEach` method is used in case you want to produce side-effects. Use the `map` method when you want to follow a functional paradigm where you return a new array and do not mutate the state of any existing objects

```C++
////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
// Lecture 148: The map method
///////////////////////////////////////////////////////////////////////////////////////////////////////////////////

// The map Method

// Suppose the given problem is to convert an array containing values in Euros to values in Dollars
const movements = [100, 150, 200, 250, -300];
const eurToUsd = 1.1;

// This is what we would do if we were using a simple for loop
const movementsUSDfor = [];
for (const mov of movements) {
    movementsUSDfor.push(mov * eurToUsd);
}
console.log(movementsUSDfor);

// But using the map we can just do this.
// the map() calls a defined callback function on each element of an array,
// and returns a new array that contains the results.
// Just like the forEach method, the callback function in the map method also accepts the same 3 parameters
// currElement, index, entire array
// Here we are making use of only the current element
const movementsUSD = movements.map(function (mov) {
    return mov * eurToUsd;
});
// Note that the returned value is an array
console.log(movementsUSD); // Prints: [110.00000000000001, 165, 220.00000000000003, 275, 330]

// We can also just use the arrow function as the callback
const movementsUSDArr = movements.map(mov => mov * eurToUsd);
console.log(movementsUSDArr); // Prints: [110.00000000000001, 165, 220.00000000000003, 275, 330]


// You can return a completely different array
const movementsDescriptions = movements.map(
    (mov, i) =>
        `Movement ${i + 1}: You ${mov > 0 ? 'deposited' : 'withdrew'} ${Math.abs(
            mov
        )}`
);

const movementDescriptions2 = movements.map(function (mov, idx) {
    if (mov > 0) {
        return `Movement ${idx + 1}: You deposited ${mov}`
    } else {
        return `Movement ${idx + 1}: You withdrew ${Math.abs(mov)}`
    }
})

console.log(movementDescriptions2);
// Prints:
// ["Movement 1: You deposited 100", "Movement 2: You deposited 150", "Movement 3: You deposited 200", "Movement 4: You deposited 250", "Movement 5: You withdrew 300"]


////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
// Lecture 149: Computing Usernames
///////////////////////////////////////////////////////////////////////////////////////////////////////////////////

// Data
const account1 = {
    owner: 'Jonas Schmedtmann',
    movements: [200, 450, -400, 3000, -650, -130, 70, 1300],
    interestRate: 1.2, // %
    pin: 1111,
};

const account2 = {
    owner: 'Jessica Davis',
    movements: [5000, 3400, -150, -790, -3210, -1000, 8500, -30],
    interestRate: 1.5,
    pin: 2222,
};

const account3 = {
    owner: 'Steven Thomas Williams',
    movements: [200, -200, 340, -300, -20, 50, 400, -460],
    interestRate: 0.7,
    pin: 3333,
};

const account4 = {
    owner: 'Sarah Smith',
    movements: [430, 1000, 700, 50, 90],
    interestRate: 1,
    pin: 4444,
};

const accounts = [account1, account2, account3, account4];

// The requirement is as follows:
// Convert each of the names of the owners to their initials and return a new array
// Input: accounts
// Output: ['js', 'jd', 'stw', 'ss']
const userName = accounts.map(function (account, idx) {
    return account.owner.toLowerCase().split(' ')
        .map(function (name) {
            return name[0];
        })
        .join('');
});
console.log(userName); // Prints: ["js", "jd", "stw", "ss"]

// Rewriting the above as an arrow function
const userNameArr = accounts.map( account => {
    return account.owner.toLowerCase().split(' ')
        .map(name => name[0])
        .join('');
});
console.log(userNameArr);  // Prints: ["js", "jd", "stw", "ss"]
```

## Lecture 150: The filter method

- The `filter` method is used to return a new array that contains only those elements from the original array that have passed a specific condition.

Just like the `map` method, the `filter` method also accepts a callback function, and just like the callback function in the `map` method, the callback function in the `filter` method also accepts three `args`, namely - `currentElement`, `index`, entire array.

```JavaScript
const transactions = [200, -200, 340, -300, -20, 50, 400, -460];
const deposits = transactions.filter(amt => amt > 0);
console.log(deposits); // Prints: [200, 340, 50, 400]
// Original array is not modified in any way
console.log(transactions); // Prints: [200, -200, 340, -300, -20, 50, 400, -460]
```

## Lecture 151: The reduce method

- The `reduce` method is used to compute a single value using all the values in the original array.

- The callback function that is passed into the `reduce` method is different compared to the ones passed nto `map` or `forEach`. In the callback for the `reduce` method, the first parameter is the _accumulator_. This accumulator stores the value that will be finally returned from the `reduce` function. So, for example, if we are using the `reduce` method to calculate the sum of all the elements in an array, then the _accumulator_ would simply return the final sum.

- The `reduce` method also accepts a second argument which t=is the starting value of the _accumulator_

```JavaScript
// Note that the callback function of the reduce method accepts the 'accumulator' as the first args
// The reduce function, on the other hand, accepts a second args which is the starting value of the 'accumulator'
// In this case, we are passing in the initial value as 0, as can be seen from the second args
let values = [200, -200, 340, -300, -20, 50, 400, -460];
const sum = values.reduce(function (acc, currElmnt, idx, arr) {
    console.log(`In Iteration ${idx}: acc is: ${acc}`);
    return acc + currElmnt;
}, 0);
console.log(sum); // Returns: 10
/*
In Iteration 0: acc is: 0
In Iteration 1: acc is: 200
In Iteration 2: acc is: 0
In Iteration 3: acc is: 340
In Iteration 4: acc is: 40
In Iteration 5: acc is: 20
In Iteration 6: acc is: 70
In Iteration 7: acc is: 470
 */

// Now you can see the difference when we pass in a non-zero value as the starting point of the accumulator
const sum2 = values.reduce(function (acc, currElmnt, idx, arr) {
    console.log(`In Iteration ${idx}: acc is: ${acc}`);
    return acc + currElmnt;
}, 100);
console.log(sum2); // Returns: 110
/*
Prints:
In Iteration 0: acc is: 100
In Iteration 2: acc is: 100
In Iteration 3: acc is: 440
In Iteration 4: acc is: 140
In Iteration 1: acc is: 300
In Iteration 5: acc is: 120
In Iteration 6: acc is: 170
In Iteration 7: acc is: 570
 */

// Calculate the maximum value in the array
// Can you see exactly how this code is working now?????
// Calls the specified callback function for all the elements in an array.
// The return value of the callback function is the accumulated result, and is provided as an argument in the next call to the callback function.
// The reduce method calls the callbackfn one time for each element in the array.
// If initialValue is specified, it is used as the initial value to start the accumulation.
// The first call to the callbackfn function provides this value as an argument instead of an array value.
values = [200, -200, 340, -300, -20, 50, 400, -460];
const maxValue = values.reduce((maxVal, currElmnt) => {

    console.log(`Start of loop maxVal: ${maxVal}, currElmnt: ${currElmnt}`);
    // Remember that in each iteration we have to somehow return the acc so that it can be used in the next iteration
    if (maxVal < currElmnt) {
        return currElmnt;
    } else {
        return maxVal;
    }
}, values[0]);

console.log(`The max value is: ${maxValue}`);
```

## Lecture 152: The magic of chaining

- How do we debug code that is in the pipeline:

```JavaScript
// Suppose we have the following requirement:
// filter out all the -ve values
// convert values to usd
// calculate the sum
values = [200, -200, 340, -300, -20, 50, 400, -460];

const finalSum = values.filter(amt => amt > 0)
    .map(amt => amt * 1.2)
    .reduce((sum, amt) => sum + amt, 0);

console.log(finalSum);

// But now if there was an error, how would we debug?
// This is where the use case of passing the original array as an args comes in
// We can rewrite the original function as:
const finalSumDebug = values
    .filter((amt, _, arr) => {
        console.log(`Inside filter the input array is: ${arr}`);
        return amt > 0;
    })
    .map((amt, _, arr) => {
        console.log(`Inside map the input array is: ${arr}`);
        return amt * 1.2
    })
    .reduce((sum, amt, _, arr) => {
        console.log(`Inside reduce the input array is: ${arr}`);
        return sum + amt
    }, 0);

/*
The important point to note here is that on each step of the pipeline,
the array that is being passed into the next stage, is the one that was generated after the previous stage.
In this way, we can keep track of how the arrays are being modified by each step of the pipeline
Inside filter the input array is: 200,-200,340,-300,-20,50,400,-460
Inside filter the input array is: 200,-200,340,-300,-20,50,400,-460
Inside filter the input array is: 200,-200,340,-300,-20,50,400,-460
Inside filter the input array is: 200,-200,340,-300,-20,50,400,-460
Inside filter the input array is: 200,-200,340,-300,-20,50,400,-460
Inside filter the input array is: 200,-200,340,-300,-20,50,400,-460
Inside filter the input array is: 200,-200,340,-300,-20,50,400,-460
Inside filter the input array is: 200,-200,340,-300,-20,50,400,-460
Inside map the input array is: 200,340,50,400
Inside map the input array is: 200,340,50,400
Inside map the input array is: 200,340,50,400
Inside map the input array is: 200,340,50,400
Inside reduce the input array is: 240,408,60,480
Inside reduce the input array is: 240,408,60,480
Inside reduce the input array is: 240,408,60,480
```

## Lecture 155: The find method

- The `find` method returns the _element_ based on the condition that is specified in the callback function. Note that if multiple elements of the array satisfy the given condition, then in that case the `find` method will return the _first_ element that satisfies the condition.

```JavaScript
values = [200, -200, 340, -300, -20, 50, 400, -460];

// Returns the value of the first element in the array where predicate is true, and undefined otherwise.
const firstNegValue = values.find(val => val < 0);
console.log(firstNegValue); // Prints: -200
```

## Lecture 158: The findIndex method

- Returns the index of the first element in the array where predicate is true, and -1 otherwise.

```JavaScript
values = [200, -200, 340, -300, -20, 50, 400, -460];
console.log(values.findIndex(val => val === 400)); // Prints: 6
console.log(values.findIndex(val => val < 0)); // Prints: 1
console.log(values.findIndex(val => val > 500)); // Prints: -1
```

## Lecture 159: some and every method

- `some` Determines whether the specified callback function returns true for any element of an array.`every` Determines whether all the members of an array satisfy the specified test.

```JavaScript
values = [200, -200, 340, -300, -20, 50, 400, -460];
// We can search whether an element exists or not by using 'includes'
console.log(values.includes(340)); // Prints: true

// some

// The problem with this method is that it only searches for exact matches. We use the 'some' method to search for other conditions
// Suppose we want to check if a particular array contains any positive values or not
console.log(values.some(val => val >= 0)); // Prints: true

// every

// Determines whether all the members of an array satisfy the specified test.
console.log(values.every(amt => amt >= 0)); // Prints: false
values = [200, 340, 50, 400];
console.log(values.every(amt => amt >= 0)); // Prints: true
```

## Lecture 161: Sorting Arrays

- Note that the `sort` method by default converts everything to strings implicitly. Hence using the `sort` method to sort arrays of number swill lead to wrong results. Hence be sure to pass in a custom comparator when sorting numbers.

```JavaScript
// Sorting Strings
const owners = ['Jonas', 'Zach', 'Adam', 'Martha'];
console.log(owners.sort()); // Prints: ["Adam", "Jonas", "Martha", "Zach"]
// The sort() operation mutates the original array as well
console.log(owners); // Prints: ["Adam", "Jonas", "Martha", "Zach"]

// Sorting Numbers
values = [200, -200, 340, -300, -20, 50, 400, -460];
console.log(values.sort()); // Prints: [-20, -200, -300, -460, 200, 340, 400, 50]
// WHAT HAPPENED?

// Well the sort() function implicitly converts everything to string.

// So how do we sort numbers?
// We pass in a custom comparator

// Sorting in ascending order
// return>0(switch order)
// return<0(keep order)
values = [200, -200, 340, -300, -20, 50, 400, -460];
values.sort((a, b) => {
    if (a > b) {
        return 1;
    } else if (a < b) {
        return -1;
    } else {
        return 0;
    }
});

// And now the numbers are sorted in a way that makes sense
console.log(values); // Prints: [-460, -300, -200, -20, 50, 200, 340, 400]

// Sorting in descending order
values = [200, -200, 340, -300, -20, 50, 400, -460];
values.sort((a, b) => {
    if (a > b) {
        return -1;
    } else if (a < b) {
        return 1;
    } else {
        return 0;
    }
});
console.log(values); // Prints: [400, 340, 200, 50, -20, -200, -300, -460]

// If we are working with numbers, we can simplify the above operation to just this:
values = [200, -200, 340, -300, -20, 50, 400, -460];

// Ascending
values.sort((a, b) => a - b);
console.log(values); // Prints: [-460, -300, -200, -20, 50, 200, 340, 400]

// Descending
values.sort((a, b) => b - a);
console.log(values); // Prints: [400, 340, 200, 50, -20, -200, -300, -460]
```

### Lecture 162: More ways of creating and filling arrays

```JavaScript
// This is how we have been constructing arrays so far
const arr4 = [1, 2, 3, 4, 5, 6, 7];

// Note how we are using the Array() constructor to create array
console.log(new Array(1, 2, 3, 4, 5, 6, 7));

// But now note what happens when you do this:
let x = new Array(7);
// It creates an empty array of size 7 and NOT a array with a single element - 7
console.log(x); // Prints: [empty × 7]

// So the point is that when we pass in a single args to the Array() method, it creates an empty array of the specified length
// instead of creating an array of that element

// But we CAN do something useful with x
// We can use the fill() method to fill the array with elements
x.fill(5);
console.log(x); // Prints: [5, 5, 5, 5, 5, 5, 5]

// We can also specify a begin parameter in the fill() method. Now the array will be filled with 1 starting from the index 3.
x = new Array(7);
x.fill(1, 3);
console.log(x); // Prints: [empty × 3, 1, 1, 1, 1]

// We can also specify a range
x = new Array(7);
x.fill(1, 3, 5);
console.log(x); // Prints: [empty × 3, 1, 1, empty × 2]

// The starting array need not always be empty.
// Also note how we are changing the content of an array declared with const
const y = [1, 2, 3, 4, 5, 6, 7];
y.fill(5, 2, 4);
console.log(y); // Prints: [1, 2, 5, 5, 5, 6, 7]


// Array.from()


// The first args is an object that specifies the length of the array
// The second args is the callback fn that is used to create the array
const arr5 = Array.from({length: 7}, () => 5);
console.log(arr5); // [5, 5, 5, 5, 5, 5, 5]

// In the callback fn we get access to the current element and the current index
// The Array.from() method accepts 2 parameters usually:
// 1) arrayLike – An array-like object to convert to an array.
// 2) mapfn – A mapping function to call on every element of the array
const arr6 = Array.from({length: 7}, (currElmnt, idx) => idx + 1);
console.log(arr6); // Prints: [1, 2, 3, 4, 5, 6, 7]

// There are multiple things that are going on in this example
// 1) using Array.from() for creating an array from the result of querySelectorAll
// 2) document.querySelectorAll returns an 'arrayLike' structure that is first args (as shown above)
// 3) Then we are also passing in a mapping function that is oterating through each item in this arrayLike structure
//    and returning the individual numbers as an array
labelBalance.addEventListener('click', function () {
    const movementsUI = Array.from(
        document.querySelectorAll('.movements__value'),el => Number(el.textContent.replace('€', ''))
    );
    console.log(movementsUI);
    
    // This also creates an array from the document.querySelectorAll(). But then we would have to map() each element separately
    // Hence we prefer the Arrays.from() method
    const movementsUI2 = [...document.querySelectorAll('.movements__value')];
});
```