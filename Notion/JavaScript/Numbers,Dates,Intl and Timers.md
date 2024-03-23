## Converting and Checking Numbers

```JavaScript
'use strict';
// In JavaScript all numbers are represented internally as floating point numbers

console.log(23 === 23.0); // → true becuase javascript automatically converts 23 to 23.0 internally
// Base 10 -> (0 to 9). 
//(1/10 = 0.1)
// 3/10 = 3.333333
// Binary base 2 - 0 1
console.log(0.1 + 0.2); // → 0.30000000000000004;
console.log(0.1 + 0.1 === 0.3); // → false
// Conversion
console.log(Number("23")); // → 23
// When JS sees the + operator it will do type coercion
console.log(+"23"); // → 23
// Parsing - the string needs to start with a number
console.log(Number.parseInt("30px", 10)); // → 30
console.log(Number.parseInt("e23", 10)); // → NaN
console.log(Number.parseInt("2.5rem", 10)); // → 2
// parseFloat is often used with values that are coming though CSS
console.log(Number.parseFloat("2.5rem", 10)); // → 2.5

// OldSchool way of calling the global methods
// Number provides a namespace for all those methods
console.log(parseInt("5.125", 10)); // → 5
// Check if value is NaN
console.log(Number.isNaN(20)); // → false
console.log(Number.isNaN("20")); // → false
console.log(Number.isNaN(+"20X")); // → true
console.log(Number.isNaN(23 / 0)); // → false because infinity is NaN

// Best way of checking if a value is a number 
console.log(Number.isFinite(20)); // → true;
console.log(Number.isFinite("20")); // → false;
console.log(Number.isFinite(+"20X")); // → false;
console.log(Number.isFinite(23 / 0)); // → false;

// Check if value is an integer
console.log(Number.isInteger(23)); // → true;
console.log(Number.isInteger(23.0)); // → true; disscused above in the first line
console.log(Number.isInteger(23 / 0)); // → false;
```

## Math and Rounding

```JavaScript
"use strict";

// Find the square root of a number
console.log(Math.sqrt(25)); // 5
console.log(25 ** (1 / 2)); // 5

// Find the highest number
console.log(Math.max(5, 3, 18, 22, 11)); // → 22
console.log(Math.max(5, 3, 18, "22", 11)); // → 22
console.log(Math.max(5, 3, 18, "22px", 11)); // → NaN

// Find the lowest number
console.log(Math.min(5, 3, 18, 22, 11)); // → 3

// Calculate the area of a circle
console.log(Math.PI * Number.parseFloat("10px") * 2); // → 62.83185307179586

// Generate random number
console.log(Math.trunc(Math.random() * 6 + 1));
const randomInt = (min, max) =>
  Math.floor(Math.random() * (max - min) + 1) + min;
// 0...1 -> 0...(max - min) -> min...max
console.log(randomInt(10, 20));

// Rounding integers
console.log(Math.round(23.3)); // → 23
console.log(Math.round(23.9)); // → 24

console.log(Math.ceil(23.3)); // → 24
console.log(Math.ceil(23.9)); // → 24

console.log(Math.floor(23.3)); // → 23
console.log(Math.floor("23.9")); // → 23

console.log(Math.trunc(23.3)); // → 23

console.log(Math.trunc(-23.3)); // → -23
// With negative numbers rounding works the other way around
console.log(Math.floor(-23.3)); // → -24

// Rounding decimals
console.log((2.7).toFixed(0)); // → "3"
console.log((2.7).toFixed(2)); // → "2.70"
console.log((2.7516).toFixed(2)); // → "2.75"
```

## Remainder and even odd operation

```JavaScript
"use strict";

console.log(5 % 2); // → 1
console.log(5 / 2); // → 2.5, 5 = 2 * 2 + 1

console.log(8 % 3); // → 2
console.log(8 / 3); // → 2.6666666666666665, 8 = 2 * 3 + 2

console.log(6 % 2); // Even number
console.log(7 % 2); // Odd number

const isEven = (n) => n % 2 === 0;
console.log(isEven(8)); // → true
console.log(isEven(23)); // → false

// Do something on every nth time
for (let i = 0; i <= 10; i++) {
  if (i % 2 === 0) console.log(i); // → 0, 2, 4, 6, 8, 10
  if (i % 3 === 0) console.log(i); // → 0, 3, 6, 9
  if (i % 4 === 0) console.log(i); // → 0, 4, 8
}
```

## Numeric Seperator

```JavaScript
"use strict";

// 287,460,000,000
const diameter = 287_460_000_000;
console.log(diameter); // → 287460000000

const priceCents = 346_99;
console.log(priceCents); // → 34699

const transferFee1 = 15_00;
const transferFee2 = 1_500;

// const PI = 3._1415; // → SyntaxError
// const PI = 3.__1415; // → SyntaxError
// const PI = 3_.1415; // → SyntaxError
// const PI = _3.1415; // → SyntaxError

console.log(Number("230000")); // → 230000
console.log(Number("230_000")); // → NaN
```

## BIGINT

```JavaScript
"use strict";

// Biggest number that JS can represent
console.log(2 ** 53 - 1); // → 9007199254740991
console.log(Number.MAX_SAFE_INTEGER); // → 9007199254740991

// n transforms a regular number into a big int number
console.log(3514935834957345345634553453418n);
// → 3514935834957345345634553453418n
console.log(BigInt(3514935834957)); // → use it only on small numbers
// → 3514935834957n

// Operations
console.log(10000n + 10000n); // → 20000n

const huge = 1412421414124124242421421412n;
const num = 23;
console.log(huge * num);
// → TypeError: Cannot mix BigInt and other types, use explicit conversions
console.log(huge * BigInt(num)); // → 32485692524854857575692692476n

// Exceptions
console.log(20n > 15); // → true
console.log(20n === 20); // → false
console.log(typeof 20n); // → bigint
console.log(20n == 20); // → true

console.log(huge + " is REALLY big!!!");

// → 1412421414124124242421421412 is REALLY big!!!

// Divisions
console.log(10n / 3n); // → 3n
console.log(10 / 3); // → 3.3333333333333335
```

## Dates

```JavaScript
'use strict';

// Create a date
console.log(new Date());
// →Fri Jun 16 2023 13:27:55 GMT+0530 (India Standard Time) --> this is show in browser in Ndoe js js diferent is shown
let myDate = new Date();
console.log(myDate.toString()); //
console.log(myDate.toLocaleString()); //
console.log(myDate.toLocaleDateString()); //
console.log(myDate.toDateString()); //
//!Like this try different methods

//Create our own date
let CreatedDate = new Date(2023, 0, 23); //Months starts fromzero
console.log(CreatedDate.toString()); //Note that moths starts from zero
let CreatedDate1 = new Date('01-14-2023');
console.log(CreatedDate1.toString());

let myTimeStamp = Date.now();
console.log(myTimeStamp);//!in milli seconds
console.log(CreatedDate.getTime());//!convert to milli
console.log(new Date(3 * 24 * 60 * 60 * 1000));
// → Sun Jan 04 1970 02:00:00 GMT+0200 (Eastern European Summer Time)

// Working with dates
const future = new Date(2037, 10, 19, 15, 23);
console.log(future.getFullYear()); // → 2037
console.log(future.getMonth()); // → 10
console.log(future.getDate()); // → 19
console.log(future.getDay()); // → 4
console.log(future.getHours()); // → 15
console.log(future.getMinutes()); // → 23
console.log(future.getSeconds()); // → 0
/* toISOString is useful when you want to convert particular 
date into a string where you can store somewhere */
console.log(future.toISOString()); // → 2037-11-19T13:23:00.000Z
console.log(future.getTime()); // → 2142249780000;

console.log(new Date(2142249780000));
// → Thu Nov 19 2037 15:23:00 GMT+0200 (Eastern European Summer Time)

console.log(Date.now()); // → 1655446724965

future.setFullYear(2040);
console.log(future.getFullYear()); // → 2040
```

## Operations with Dates

```JavaScript
"use strict";

const future = new Date(2037, 10, 19, 15, 23);
console.log(future); // → Thu Nov 19 2037 15:23:00 GMT+0200 (EEST)
console.log(+future); // → 2142249780000

const calcDaysPassed = (date1, date2) =>
  Math.abs((date2 - date1) / (1000 * 60 * 60 * 24));
const days1 = calcDaysPassed(new Date(2037, 3, 14), new Date(2037, 3, 24));
console.log(days1); // → 10

// If you need really precise calculation -> MomentJS library
```

## Internationalizing Dates

```JavaScript
"use strict";

// Language-sensitive date and time formatting
const now = new Date();
console.log(now); // → Fri Jun 17 2022 22:04:59 GMT+0300 (EEST)
console.log(new Intl.DateTimeFormat("en-US").format(now));
// → 6/17/2022
console.log(new Intl.DateTimeFormat("en-GB").format(now));
// → 17/06/2022
console.log(new Intl.DateTimeFormat("bg").format(now));
// → 17.06.2022 г.

const options = {
  hour: "numeric",
  minute: "numeric",
  day: "numeric",
  month: "long",
  year: "numeric",
  weekday: "long",
};
console.log(new Intl.DateTimeFormat("en-GB", options).format(now));
// → Friday, 17 June 2022, 22:08
console.log(new Intl.DateTimeFormat("bg", options).format(now));
// → петък, 17 юни 2022 г., 22:09 ч.

// Get the preferred language of the user, usually the language of the browser UI.
const locale = navigator.language;
console.log(locale); // → "bg"

// ISO Language Code Table (en-US, en-GB, bg, ...)
```

## Internationalizing Numbers

```JavaScript
"use strict";

const num = 3884764.23;
console.log(`US: ${new Intl.NumberFormat("en-US").format(num)}`);
// → US: 3,884,764.23

console.log(`Germany: ${new Intl.NumberFormat("de-DE").format(num)}`);
// → Germany: 3.884.764,23

console.log(`Bulgaria: ${new Intl.NumberFormat("bg").format(num)}`);
// → Bulgaria: 3 884 764,23

const options = {
  style: "unit",
  unit: "mile-per-hour",
};
console.log(`UK: ${new Intl.NumberFormat("en-GB", options).format(num)}`);
// → UK: 3,884,764.23 mph

console.log(`Greek: ${new Intl.NumberFormat("el", options).format(num)}`);
// → Greek: 3.884.764,23 μίλια/ώρα

const options2 = {
  style: "currency",
  unit: "mile-per-hour",
  currency: "EUR",
  // useGrouping: false
};
console.log(`US: ${new Intl.NumberFormat("en-US", options2).format(num)}`);
// → US: €3,884,764.23

console.log(`Bulgaria: ${new Intl.NumberFormat("bg", options2).format(num)}`);
// → Bulgaria: 3884764,23 €
```

## Timeout Function

```JavaScript
"use strict";

/* The setTimeout method sets a timer which executes a function
or specified piece of code once the timer expires. */
setTimeout((ing1, ing2) => {
  console.log(`Here is your pizza with ${ing1} and ${ing2} 🍕`);
}, 3000, "olives", "spinach");
console.log("Waiting...");
// → Waiting...
// 3s → Here is your pizza with olives and spinach 🍕

const ingredients = ["olives", "spinach"];
const pizzaTimer = setTimeout((ing1, ing2) => {
  console.log(`Here is your pizza with ${ing1} and ${ing2} 🍕`);
}, 3000, ...ingredients);
console.log("Waiting...");
// → Waiting...
// 3s → ""

// Cancel setTimeout method
if (ingredients.includes("spinach")) clearTimeout(pizzaTimer);

/* The setInterval method, repeatedly calls a function or executes
a code snippet, with a fixed time delay between each call. */
setInterval(() => {
  const now = new Date();
  console.log(now.toLocaleTimeString());
}, 1000)
// 1s → 09:14:00 ч.
// 1s → 09:14:01 ч.
// 1s → 09:14:02 ч.
// 1s → 09:14:03 ч.
// 1s → ...

/* Using setInterval is a bad practice because at higher loads the
interval is not accurate. It is correct to use setTimeout recursively. */
```