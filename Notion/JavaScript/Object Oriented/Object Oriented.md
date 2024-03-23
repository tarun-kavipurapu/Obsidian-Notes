## What is OOP

![[Images-attachments/Untitled 26.png|Untitled 26.png]]

![[Images-attachments/Untitled 1 6.png|Untitled 1 6.png]]

![[Images-attachments/Untitled 2 5.png|Untitled 2 5.png]]

![[Images-attachments/Untitled 3 4.png|Untitled 3 4.png]]

![[Images-attachments/Untitled 4 4.png|Untitled 4 4.png]]

![[Images-attachments/Untitled 5 4.png|Untitled 5 4.png]]

## OOP in Javascript

![[Images-attachments/Untitled 6 4.png|Untitled 6 4.png]]

![[Images-attachments/Untitled 7 4.png|Untitled 7 4.png]]

## Implement OOP in javascript

```JavaScript
/* The only difference between a regular function and a
constructor function is that we call the new operator. */

// There is a convention that constructor functions should start with a capital letter.
const Person = function (firstName, birthYear) {
  // Instance properties      
  this.firstName = firstName;
  this.birthYear = birthYear;

  /* Never create a method inside a constructor function, because a 
  new copy of the method is created on each new instance. and thiswilll effect the performance of the progrram*/
  this.calcAge = function () {
    return 2037 - this.birthYear;
  }
}

const jonas = new Person("Jonas", 1991);
console.log(jonas); // → Person {firstName: 'Jonas', birthYear: 1991}

/* What happens when we call a function with the new operator.
1. A new empty object is created
2. The function is called and the this keyword is set to the newly created object
3. The new object is linked (__proto__ property) to the constructor function's prototype property
4. The function implicitly returns the object that we created */

/* The instanceof operator allows us to check whether
an object belongs to a certain constructor function. */
console.log(jonas instanceof Person); // → true

/* Function constructors are not really a feature of the JS language, they
are simply a pattern that was created by other developers. */
```

Never Create a Method inside a constructor function because each time a new instance of a constructor function is created, the method is also created again so this will ultimately affect the performance of the program.

## Prototypes

```JavaScript
console.log(jonas.calcAge()); // → 46

console.log(jonas); // → Person {firstName: 'Jonas', birthYear: 1991}
console.log(jonas.__proto__); // → {calcAge: ƒ, constructor: ƒ}
console.log(jonas.__proto__.constructor); // points to the constructor function

/* Person.prototype is not the prototype of Person, it is what is going to be used as the
prototype of all the objects that are created with the Person constructor function. */
console.log(jonas.__proto__ === Person.prototype); // → true
console.log(Person.prototype.isPrototypeOf(jonas)); // → true

// .prototype should've been named .prototypeOfLinkedObjects to be more clear.

// We can also set properties to the prototype
Person.prototype.species = "Homo Sapiens";
// species property is not directly in the object, so it's not it's own property
console.log(jonas.species); // → Homo Sapiens
console.log(jonas.__proto__); // → {calcAge: ƒ, species: 'Homo Sapiens', constructor: ƒ}

console.log(jonas.hasOwnProperty("firstName")); // → true
console.log(jonas.hasOwnProperty("species")); // → false
```

Prototypes are not created for every instance of a constructor function hence they are not the own properties of a class or constructor function we use prototypes so that we can use them only when necessary for certain instances. Read the code for more clarity

Step No:3 links the `.__proto__` —>property of the object to `Person.prototype` of the constructor function

## Working Of Prototypal inheritance and the prototype chain

![[Images-attachments/Untitled 8 4.png|Untitled 8 4.png]]

![[Images-attachments/Untitled 9 3.png|Untitled 9 3.png]]

![[Images-attachments/Untitled 10 3.png|Untitled 10 3.png]]

## Prototypal Inheritance on Built-In Objects

```JavaScript
const Person = function (firstName, birthYear) {
  this.firstName = firstName;
  this.birthYear = birthYear;
};

const jonas = new Person("Jonas", 1991);

console.log(jonas.__proto__); // → Jonas.prototype
console.log(jonas.__proto__.__proto__); // → Object.prototype
console.log(jonas.__proto__.__proto__.__proto__); // → null
console.dir(Person.prototype.constructer)//points back to Person

const arr = [7, 5, 1, 5, 7, 5]; // new Array === []
console.log(arr.__proto__); // → Array.prototype
console.log(arr.__proto__ === Array.prototype); // → true

/* All prototypes are objects that's why the array
prototype has an object prototype linked to it */
console.log(arr.__proto__.__proto__); // → Object.prototype

/* In JavaScript, arrays are objects, where the position of a value
in the array (its numerical index) is its key. They also have their
own array prototype, which has various methods and properties that
normal objects don't have, such as push and pop, length, indexOf, etc. */
console.log(arr instanceof Object); // → true

// It's not a good idea to extend the prototype of a build in object 
//below is not a good practice to do 
Array.prototype.unique = function () {
  // The this keyword will point to the array that the method has been called on
  return [...new Set(this)];
}

console.log(arr.unique()); // → [5, 1, 7]
```

## ES6 Classes

```JavaScript
// Class expression
const PersonE = class {};

// Class declaration
class PersonD {
  constructor(firstName, birthYear) {
    this.firstName = firstName;
    this.birthYear = birthYear;
  }

  // Methods will be added to .prototype property
  calcAge() {
    console.log(2037 - this.birthYear);
  }
}

const jessica = new PersonD("Jessica", 1996);
jessica.calcAge(); // → 41

console.log(jessica.__proto__ == PersonD.prototype); // → true

// What happens behind the scenes when you add a method inside the class
PersonD.prototype.greet = function () {
  console.log(`Hey ${this.firstName}`);
}

jessica.greet(); // → Hey Jessica

/* Things to know about classes
1. Classes are NOT hoisted even if they are class declarations
2. Classes are first-class citizens (they are treated as variables)
3. Classes are always executed in strict mode */
```

  

## Setters and Getters

In JavaScript, getter and setter are special functions that allow you to define the behavior of accessing and modifying object properties. They provide a way to control the access and assignment of values to an object's properties, allowing you to add additional logic or validations.

Getter:  
A getter is a function that is used to retrieve the value of a specific property. It allows you to define custom behavior when reading the value of an object's property. To define a getter, you use the  
`get` keyword followed by the property name.

Here's an example of a getter in JavaScript:

```Plain
const person = {
  firstName: "John",
  lastName: "Doe",

  get fullName() {
    return this.firstName + " " + this.lastName;
  }
};

console.log(person.fullName); // Output: "John Doe"
```

In the example above, `fullName` is a getter function defined within the `person` object. When `person.fullName` is accessed, the getter function is automatically invoked, and it returns the concatenated full name.

Setter:  
A setter is a function that is used to assign a value to a specific property. It allows you to define custom behavior when assigning a value to an object's property. To define a setter, you use the  
`set` keyword followed by the property name.

Here's an example of a setter in JavaScript:

```Plain
const person = {
  firstName: "John",
  lastName: "Doe",

  set fullName(name) {
    const [firstName, lastName] = name.split(" ");
    this.firstName = firstName;
    this.lastName = lastName;
  }
};

person.fullName = "Jane Smith";
console.log(person.firstName); // Output: "Jane"
console.log(person.lastName); // Output: "Smith"
```

In the example above, `fullName` is a setter function defined within the `person` object. When `person.fullName` is assigned a value, the setter function is automatically invoked, splitting the name into first and last names and updating the respective properties.

Getters and setters provide an abstraction over object properties, allowing you to implement custom logic and control access to the properties. They are useful when you want to perform additional operations, such as validation or formatting, when getting or setting a property value.

**For classes**

```JavaScript
'use strict';
class PersonCl {
  constructor(fullName, birthYear) {
    this.fullName = fullName;
    this.birthYear = birthYear;
  }

  // Instance methods
  // Methods will be added to .prototype property
  calcAge() {
    console.log(2037 - this.birthYear);
  }

  greet() {
    console.log(`Hey ${this.fullName}`);
  }

  get age() {
    return 2037 - this.birthYear;
  }
  set fullName(name) { 
    if(name.includes(' ')) this._fullName = name;
    else alert(`${name} is not a full name `);
  }
  get fullName() {
    return this._fullName;
  }
}
const obj = new PersonCl('Tarun',2004);
console.log(obj.age);
```

## Static Methods

In JavaScript, a static function is a function that belongs to the class itself rather than an instance of the class. It is defined on the class constructor function or class itself, rather than on the prototype of the class. Static functions are also sometimes referred to as "class methods" because they are associated with the class itself rather than with instances of the class.

Here's an example that demonstrates how to define and use static functions in JavaScript:

**These are Not attached to prototype hence they are non-inheritable to the object from class.**

```JavaScript
class Circle {
  constructor(radius) {
    this.radius = radius;
  }

  // Instance method
  calculateArea() {
    return Math.PI * this.radius * this.radius;
  }

  // Static method
  static createDefaultCircle() {
    return new Circle(1);
  }
}

const circle = new Circle(5);
console.log(circle.calculateArea()); // Output: 78.53981633974483

const defaultCircle = Circle.createDefaultCircle();
console.log(defaultCircle.radius); // Output: 1
console.log(defaultCircle.calculateArea()); // Output: 3.141592653589793
```

In the example above, the `Circle` class has two methods: `calculateArea()` and `createDefaultCircle()`. The `calculateArea()` method is an instance method, meaning it is defined on the prototype of the `Circle` class and can be called on instances of the class. The `createDefaultCircle()` method is a static method, defined using the `static` keyword. It is associated with the `Circle` class itself and can be called directly on the class without creating an instance.

Static functions are useful when you have functionality that doesn't depend on any specific instance data but is related to the class as a whole. They can be used for utility functions, factory methods, or any other functionality that doesn't require access to instance-specific data.

It's important to note that static functions cannot access instance-specific data or methods since they are not invoked on instances of the class. They can only access other static members of the class.

In JavaScript, the `**Array**` class has several built-in static methods that allow you to perform operations on arrays. Here are a few examples:

1. `**Array.from()**`:  
    The  
    `**Array.from()**` method creates a new array from an iterable object or array-like object. It allows you to convert array-like or iterable objects into actual arrays.

```JavaScript
javascriptCopy code
const arrayLike = { 0: 'a', 1: 'b', 2: 'c', length: 3 };
const newArray = Array.from(arrayLike);
console.log(newArray); // Output: ['a', 'b', 'c']
const arr = [1,2,3]
arr.from(arrayLike)//--> Not valid since .from is static function and attached to Array class rather than arr object. 
```

## Object.create()

![[Images-attachments/Untitled 11 3.png|Untitled 11 3.png]]

![[Images-attachments/Untitled 12 3.png|Untitled 12 3.png]]