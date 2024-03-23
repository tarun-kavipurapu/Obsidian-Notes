Learn About (e).

![[Images-attachments/Untitled 25.png|Untitled 25.png]]

## Lecture 71: What is the DOM and DOM Manipulation

- The DOM and DOM Methods like `document.querySelector()` are NOT a part of the JS language. Instead, they are a part of the Web APIs. The Web APIs are libraries that the browser implements - and we can access these libraries through our JS code. These web APIs are also written in JS. There is a separate DOM specification that each browser implements. Just like DOM, there are other Web APIs as well, like - Timers, Fetch, etc.The HTML corresponding to the below JS can be seen [here](https://rgbk21.github.io/Code/JavaScript/imgs/UdemyCourse/Module7/index.html). This is not the functioning JS. It is more for your reference about how to use different aspects of the language.

```JavaScript
'use strict';

// This is how you select elements using JS
// The argument being passed into the querySelector follows the same format that you
// would have used in case you were using CSS selectors. i.e.


// In order to select a class named 'message':
console.log(document.querySelector('.message')); // Prints: <p class="message">Start guessing...</p>

// In order to select an ID named 'heading':
console.log(document.querySelector('\#heading')); // Prints: <p class="message">Start guessing...</p>

// You can also select an element by id as follows. Note, now we do not need the # symbol to precede the class name
console.log(document.getElementById('heading')); // Prints: <p class="message">Start guessing...</p>

// In order to select the <p> element:
// Note that this will select ONLY the very FIRST <p> element that it encounters
console.log(document.querySelector('p'));

// If you want to select EVERY <p> element:
// For now you can assume that NodeList is an array
console.log(document.querySelectorAll('p')); // Prints: NodeList(4) [p.between, p.message, p.label-score, p.label-highscore]
// You can loop through each element
const allElements = document.querySelectorAll('p');
for (let i = 0; i < allElements.length; i++) {
    // You can get the text content within each <p>
    console.log(allElements[i].textContent);
}
/*
 The above for loop Prints:
 (Between 1 and 20)
 Start guessing...
 Score: 20
 Highscore: 0
*/

// Similar to getting the test content, we can also set the text content of an element through JS
document.querySelector('.message').textContent = "Another Message now!";
console.log(document.querySelector('.message')); // Prints: <p class="message">Another Message now!</p>

// Note the 'input' element in the HTML page.
// In order to read or write the value contained in the input element, we use the 'value' property
console.log(document.querySelector('.guess').value);
document.querySelector('.guess').value = 21;
console.log(` After change: ${document.querySelector('.guess').value}`); // Prints: After change: 21

// *********************************************************************************************************************
// Lecture 73: Handling Click Events
// *********************************************************************************************************************

// In order to listen for events, we first need to select the element on which the event should happen
// In this particular case, suppose we want to listen for a click on the 'Check!' button
// Note that the button has two classes applied to it - 'btn' and 'check'.
// The 'btn' class is a general class that is applied to another button 'Again!' as well.
// Hence we need a more specific class than that. Hence we select the 'check' class

// So that is what we are doing here - on the button with the class 'check', we are adding an event listener
// that is going to be listening for a 'click' event, and when that event is clicked
// it is going to execute the function that is passed in as the second args.
// The function is just going to read the value from the 'input' element and log it to the console.
// Note that the function will NOT be called as soon as the addEventListener() code is executed.
// Rather the function will be called internally by the JS engine only once the element has been clicked.
document.querySelector('.check').addEventListener('click', function () {
    // We are also converting the value to a number type because the default type that is read from the input is string
    const guessedValue = Number(document.querySelector('.guess').value);
    console.log(guessedValue);
});


// *********************************************************************************************************************
// Lecture 75: Manipulating CSS Styles
// *********************************************************************************************************************

// How do we change the style of an element?
// For instance, supposed we wanted to change the background color of the page when a certain button is clicked
document.querySelector('.change-bg-color').addEventListener('click', function () {
    // First we are selecting the element whose style we want to change. In this case, we want to change the
    // background color of the entire page, hence we first select the entire page.
    // Then we access the 'style' property on the element
    // And then we access the 'backgroundColor' property on style.p Note the camel case.
    // All CSS properties that are written in kebab case chagne to camelCase in JS.
    // Hence we access the background-color property in CSS using the backgroundColor key in JS.
    // Properties are set by passing the values as strings
    document.querySelector('body').style.backgroundColor = "red";

    // And suppose we also wanted to change the width of the 'guess' class
    // Note that even though we are dealing with sizes, which are typically numbers,
    // we are still passing in the value as a string, not as a number
    document.querySelector('.number').style.width = '30rem';
})
```

This is the Complete JS for the ImageGallery page [here](https://rgbk21.github.io/ImageGallery/ImageGallery.html).

```JavaScript
'use strict';

// The overarching understanding is this - in CSS we basically have classes that contain all of the styling that is required for
// a particular element. In order to modify the DOM, what we are doing is we are adding or removing the 'hidden' class from the list
// of classes associated with the element.
// This allows us to modify the DOM

// Using variables to store the elements so that we do not have to select the element every time
const imgs = document.querySelectorAll('.my-img');
const overlay = document.querySelector('.overlay');
const modal = document.querySelector('.my-modal');
const fullImage = document.querySelector('\#full-image');
// const closeModalBtn = document.querySelector('.close-modal');

const openFullImageOverlay = function () {
    // we are removing the 'hidden' class that will cause the element to appear on the viewport
    // Note: although we are removing the class 'hidden', we are passing in the args as 'hidden' and not '.hidden'
    // So there is no dot before the class name
    overlay.classList.remove('hidden');
    modal.classList.remove('hidden');
    fullImage.classList.remove('hidden');
    // Note how we are using 'this' to access the img element that has been clicked
    fullImage.src = this.src;
    console.log(fullImage.src);

    // When it comes to removing or adding classes, we can add/removing multiple classes at the same time
    // The list of classes is passed comma separated. Eg.
    //     modal.classList.remove('hidden', 'class1', 'class2');
};

const closeFullImageOverlay = function () {
    overlay.classList.add('hidden');
    modal.classList.add('hidden');
    fullImage.classList.add('hidden');
};

for (let i = 0; i < imgs.length; i++) {
    // Adding an evenListener to each element.
    // Note that we are not doing openFullImageOverlay(), ie. we are not calling the function
    // We are just passing in the name of the function and telling JS to call it once the element is clicked.
    imgs[i].addEventListener('click', openFullImageOverlay);
}

// Close the overlay when the Esc key is pressed
// Note that the event is applied on the ENTIRE document - i.e. DOM - they are hence known as Global events,
// because they do not happen on one specific element.
// There are 3 types of events associated with keypresses - keydown, keypress, keyup. We mostly use keydown.
// Also note how we are passing in the 'event' that is passed in by JS. JS, when calling this function,
// is going to pass in the event as an argument.
document.addEventListener('keydown', function (event) {
    // Note how we are checking if the classList contains a specific class here
    // In this case, we want to close the overlay, only if it is not currently hidden.
    // Also note how we are reading the key that was pressed by using the 'e.key' property
    if (event.key === 'Escape' && !overlay.classList.contains('hidden')) {
        closeFullImageOverlay();
    }
    
    // Alternatively, we can also use. This method adds the class to the classlist if it is not present
    // and removes the class from the classlist if it is present
    // overlay.classList.toggle('hidden');

    //Just for reference:
    console.log(event);
});

// Close the overlay when the user clicks outside the image
overlay.addEventListener('click', closeFullImageOverlay);

// Close the overlay when the user clicks the cross button
// Cross button is removed from HTML because UGHH..
// closeModalBtn.addEventListener('click', closeFullImageOverlay);
```

- TODO: How does the CSS on the Pig Game actually work?
- This is the Complete JS for the Pig Game page [here](https://rgbk21.github.io/Pig_Game/index.html)

```JavaScript
'use strict';

let currPlayer = 0;
let playerScore = 0;
let overallScorePlayer0 = 0;
let overallScorePlayer1 = 0;
let targetWinValue = 20;
let gameInProgress = true;

// Selecting the elements as that we do not have to select the elements separately every time
const overallPlayer0ScoreElmnt = document.querySelector('\#score--0');
const overallPlayer1ScoreElmnt = document.querySelector('\#score--1');
const currScorePlayer0Elmnt = document.querySelector('\#current--0');
const currScorePlayer1Elmnt = document.querySelector('\#current--1');
const player0Elmnt = document.querySelector('.player--0');
const player1Elmnt = document.querySelector('.player--1');
const diceElmnt = document.querySelector('.dice');

const resetPage = function () {
    overallPlayer0ScoreElmnt.textContent = '0';
    overallPlayer1ScoreElmnt.textContent = '0';
    currScorePlayer0Elmnt.textContent = '0';
    currScorePlayer1Elmnt.textContent = '0';
    diceElmnt.classList.add('hidden');
    player0Elmnt.classList.remove('player--winner');
    player0Elmnt.classList.add('player--active');
    player1Elmnt.classList.remove('player--winner', 'player--active');
    currPlayer = 0;
    playerScore = 0;
    overallScorePlayer0 = 0;
    overallScorePlayer1 = 0;
    gameInProgress = true;
};

// Reset the page to remove garbage values when the page is first loaded
resetPage();

document.querySelector('.btn--roll').addEventListener('click', function () {

    // gameInProgress amkes sure that once the user has won, all the buttons on the page have been disabled
    if (gameInProgress) {

        // Generate a dice roll
        let diceRollVal = Math.trunc(Math.random() * 6) + 1;
        console.log(`Dice roll value is ${diceRollVal}`);

        // Change the dice image that is visible
        if (diceElmnt.classList.contains('hidden')) {
            diceElmnt.classList.remove('hidden');
        }
        diceElmnt.src = `dice-${diceRollVal}.png`;
        console.log(`Image loaded: dice-${diceRollVal}.png`);

        if (diceRollVal !== 1) {
            // Add the value of dice roll to the score of the curr player
            if (currPlayer === 0) {
                playerScore += diceRollVal;
                currScorePlayer0Elmnt.textContent = playerScore;
            } else {
                playerScore += diceRollVal;
                currScorePlayer1Elmnt.textContent = playerScore;
            }
        } else {
            // curr player rolled a 1
            // change the current score to 0 for the curr player
            playerScore = 0;

            if (currPlayer === 0) {
                currScorePlayer0Elmnt.textContent = '0';
                // and switch player
                currPlayer = 1;
                player0Elmnt.classList.toggle('player--active');
                player1Elmnt.classList.toggle('player--active');
            } else {
                currScorePlayer1Elmnt.textContent = '0';
                currPlayer = 0;
                player0Elmnt.classList.toggle('player--active');
                player1Elmnt.classList.toggle('player--active');
            }
        }
    }
});

document.querySelector('.btn--hold').addEventListener('click', function () {

    if (gameInProgress) {

        if (currPlayer === 0) {
            overallScorePlayer0 += playerScore;
            if (overallScorePlayer0 >= targetWinValue) {
                overallPlayer0ScoreElmnt.textContent = overallScorePlayer0;
                currScorePlayer0Elmnt.textContent = '0';
                declareVictory();
            } else {
                // Pass turn over to Player 1
                overallPlayer0ScoreElmnt.textContent = overallScorePlayer0;
                currScorePlayer0Elmnt.textContent = '0';
                playerScore = 0;
                currPlayer = 1;
                player0Elmnt.classList.toggle('player--active');
                player1Elmnt.classList.toggle('player--active');
            }
        } else {
            overallScorePlayer1 += playerScore;
            if (overallScorePlayer1 >= targetWinValue) {
                overallPlayer1ScoreElmnt.textContent = overallScorePlayer1;
                currScorePlayer1Elmnt.textContent = '0';
                declareVictory();
            } else {
                // Pass turn over to Player 0
                overallPlayer1ScoreElmnt.textContent = overallScorePlayer1;
                currScorePlayer1Elmnt.textContent = '0';
                playerScore = 0;
                currPlayer = 0;
                player0Elmnt.classList.toggle('player--active');
                player1Elmnt.classList.toggle('player--active');
            }
        }
    }
});

// Start a new game
document.querySelector('.btn--new').addEventListener('click', function () {
    resetPage();
});

const declareVictory = function () {

    gameInProgress = false;
    diceElmnt.classList.add('hidden');

    if (currPlayer === 0) {
        player0Elmnt.classList.add('player--winner');
    } else {
        player1Elmnt.classList.add('player--winner');
    }
};
```

## Node List and HTML Collection

  

```JavaScript
document.querySelectorAll('li') //return a node list 
//NodeList(6) [li.main-nav-link, li.main-nav-link, li.main-nav-link, li.main-nav-link, li.main-nav-link, li.main-nav-link]
```

Node that this is not a array list but a Node list this Node list has Different Properties than the array list.

![[Images-attachments/Untitled 1 5.png|Untitled 1 5.png]]

Hence we cannot use array method like map and find. but forEach can be applied

![[Images-attachments/Untitled 2 4.png|Untitled 2 4.png]]

Convert Node list and HTML collection to Array

![[Images-attachments/Untitled 3 3.png|Untitled 3 3.png]]

Now properties of Arrays can be applied

## How DOM Really Works

![[Images-attachments/Untitled 4 3.png|Untitled 4 3.png]]

Yes, the Document Object Model (DOM) represents an HTML or XML document as a structured object model. It provides a way to interact with the document programmatically using objects, properties, and methods.

When a web browser loads an HTML document, it parses the HTML code and creates a hierarchical representation of the document known as the DOM tree. This tree structure represents the structure and content of the HTML document, where each element, attribute, and text node becomes an object in the DOM.

For example, consider the following HTML snippet:

```HTML
<!DOCTYPE html>
<html>
  <head>
    <title>Example Page</title>
  </head>
  <body>
    <h1>Hello, world!</h1>
    <p>This is an example page.</p>
  </body>
</html>
```

When this HTML is loaded by a web browser, it creates a corresponding DOM structure. It would look something like this:

```Plain
Document
  └── DocumentType (<!DOCTYPE html>)
  └── Element (html)
       └── Element (head)
       │    └── Element (title)
       │         └── TextNode ("Example Page")
       └── Element (body)
            └── Element (h1)
            │    └── TextNode ("Hello, world!")
            └── Element (p)
                 └── TextNode ("This is an example page.")
```

In this representation, each HTML element, such as `<html>`, `<head>`, `<title>`, `<body>`, `<h1>`, `<p>`, is converted into a corresponding DOM object. The text content within the elements becomes TextNodes in the DOM.

Once the HTML is converted to a DOM object, you can use JavaScript to interact with and manipulate the elements, attributes, and content of the document. This allows you to dynamically update and modify the HTML structure and behavior based on user interactions or other events.

![[Images-attachments/Untitled 5 3.png|Untitled 5 3.png]]

Here all the Children inherit the properties of parent nodes through inheritance,

Also Point to be noted here is that this is not a node tree.

## Some Basic methods in Dom that we must know

If we use <a> element for a popup we need to put something to the link like “#”. This method has its problem because when we click the button the page scrools to the top. How can we avoid it? By preventing the default.

```JavaScript
const openModal = function (e) {
e.preventDefault();
modal.classList.remove(‘hidden’);
overlay.classList.remove(‘hidden’);
};
```

# **Creating DOM Elements**

For the navigation menu we have a special element in html

```HTML
<nav> … <nav />
```

To add an html element we first need to create it with template literals ``

After changing values dynamically with template literals we need to add it into our document using **insertAdjacentHTML**

Here we have few options specifying where to inject the element

![[Images-attachments/Untitled 6 3.png|Untitled 6 3.png]]

```JavaScript
const displayMovements = function (movements) {
movements.forEach(function (mov, idx) {
const type = mov > 0 ? ‘deposit’ : ‘withdrawal’;
const html = `
<div class=”movements__row”>
<div class=”movements__type movements__type — ${type}”>${idx} ${type}</div>
<div class=”movements__date”>3 days ago</div>
<div class=”movements__value”>${mov}</div>
</div>
`;
containerMovements.insertAdjacentHTML(‘afterbegin’, html);
});
};
displayMovements(account1.movements);
```

However we have a small problem we still have some html coming from the hard-coded part we need to somehow reset the movement container before we start adding new ones.

**innerHTML does the job**

```JavaScript
const displayMovements = function (movements) {containerMovements.innerHTML = ‘’;};
```

## Selecting Creating and Deleting Elements//IMPORTANT for future reference

  

```JavaScript
/* • Allows us to make JavaScript interact with the browser;
• We can write JavaScript to create, modify and delete HTML elements; set styles, classes and attributes; and listen and respond to events
• DOM tree is generated from an HTML document, which we can then interact with;
• Dom is very complex API that contains lots of methods and properties to interact with the DOM tree; */

// --- Selecting elements ---
// Selects the <html> tag
//If you want to select the whole document. Selecting document is not enough
document.documentElement;

// Selects the <head> tag
document.head;

// Selects the <body> tag
document.body;

// Uses CSS selector to select the first matching element 
document.querySelector(".header");

// Uses CSS selector to select one or more elements and returns a NodeList collection
document.querySelectorAll(".section");

// Selects an individual element using a specific id
document.getElementById("section--1");

// Selects all elements with specific tag name and returns a HTMLCollection
document.getElementsByTagName("buttons");
//Also an important thing to note here is that HTML collection is updated in real time whereas the NODE list is a static list.

// Selects all elements with a given class name and returns a HTMLCollection
document.getElementsByClassName("btn");


// --- Creating and inserting elements ---
// Inserts a text as nodes into the DOM tree at a specified position
sel.insertAdjacentHTML(position, text);

// Create, modify and prepend element
const message = document.createElement("div");
message.classList.add("cookie-message");
// message.textContent = "We use cookies for improved functionality and analytics.";
message.innerHTML = `
We use cookies for improved functionality and analytics.
<button class='btn btn--close--cookie'>Got it!</button>
`;
// Add as first child
sel.prepend(message);
// Add as last child (moves the element from first child to last child)
sel.append(message);

// (!) The DOM element message is unique, it can only exist in one place at a time

// Copy an element and all its child elements (prepends a new copy of the message element)
sel.prepend(message.cloneNode(true));

// Add element before or after an element as a sibling
sel.before(message);
sel.after(message)


// --- Delete elements ---
document.querySelector(".btn--close-cookie").addEventListener("click", () => {
  // New way of removing element
  message.remove();

  // Old way of removing element
  message.parentElement.removeChild(message);
});
```

We have other selection methods than the querySelector for example TagName

**Explaining Dynamic nature of HTML Collection**

```JavaScript
const allButtons = document.getElementsByTagName(‘button’);
console.log(allButtons);
```

This returns an HTML Collection, this is something different than a NodeList. The biggest difference is HtmlCollection change as the DOM changed. For example let’s say I delete a button from the DOM how would my Html Collection change

![[Images-attachments/Untitled 7 3.png|Untitled 7 3.png]]

The same thing doesn't happen with NodeList because simply NodeList is created before the element is deleted.

We can use

- getElementById(‘myid’);
- getElementByClassName(‘myclass’);

These also return a htmlcollection just like the TagName. Beware that we don’t have to use ‘.’ or ‘#’ for these ones.

  

  

## Styles attributes and classes//IMPORTANT for future reference

**When we define a style in the js. They are added to the code as inline styles.**

**CSS Variables:**

You can define variables to the vanilla css, you wonder how to do it? Like that.

```Plain
:root { — main-bg-color: brown;}
```

Copy and SaveShare

We use document root for selection because it scopes all over the app. Its equivalent in js is document.documentElement

and to use in somewhere we use:

```Plain
element {background-color: var( — main-bg-color);}
```

Copy and SaveShare

Simple as that. They are called css selection properties but they are same as variables.

```JavaScript
// --- Styles ---
const message = document.createElement("div");
message.style.backgroundColor = "\#37383d";
message.style.width = "120px";

// Get an style that was set explicitly
console.log(message.style.color); // → empty (the color hasn't been set explicitly)
console.log(message.style.backgroundColor); // → rgb(55, 56, 61) this only returns for styles that are set in Inline and DOM sets the styles in Inline manner.

// Get a style that was set implicitly (.css file)
console.log(getComputedStyle(message).color); // →  returns the color
console.log(getComputedStyle(message).height); // →  returns the height

// getComputedStyle.height returns a string - 0px
message.style.height = Number.parseFloat(getComputedStyle(message).height, 10) + 40 + "px";


// --- Working with CSS Custom Properties ---
// Set or change an CSS custom property
document.documentElement.style.setProperty("--color-primary", "orange");


// --- Attributes ---
const logo = document.querySelector(".img");
const link = document.querySelector(".link");
// Standard attributes
console.log(logo.alt);
console.log(logo.src);
console.log(logo.className);
logo.alt = "Beautiful minimalist logo";

// Non-standard attributes
console.log(logo.designer); // won't work
console.log(logo.getAttribute("designer")); // works
logo.setAttribute("company", "Bankist");

// Absolute vs Relative
console.log(logo.src); // → http://127.0.0.1:8080/img/logo.png (absolute version)
console.log(logo.getAttribute("src")); // → img/logo.png (relative version)

console.log(link.href); // → http://127.0.0.1:8080/# (absolute version)
console.log(link.getAttribute("href")); // → # (relative version)

// Data attributes (special attributes that start with data)
// data-version-number
console.log(sel.dataset.versionNumber);
// data-full-name
console.log(sel.dataset.fullName);


// --- Classes ---
sel.classList.add("a", "b", "c");
sel.classList.remove("a", "b", "c");
sel.classList.toggle();
sel.classList.contains();

// Don't use because it overrides all existing classes
sel.className = "a";
```

  

learnt a bit about how to make the website scroll t a certain section when a button is clicked this can always be searched in GPT

## Types of Events and Event Handlers

```JavaScript
const h1 = document.querySelector("h1");

function func() {
  alert("Event triggered!");

  // Remove event listener
  h1.removeEventListener("mouseenter", func);
}

// New way of adding events
h1.addEventListener("mouseenter", func);
// Using addEventListener we can attach multiple events
h1.addEventListener("click", func);
 //Event listner can also be removed using 
h1.removeEventListener("mouseenter", func);

Executing an Event after some amount of time 
setTimeout(()=>h1.removeEventListener("mouseenter", func),3000)//after 3 seconds remove event 


// Old way of adding events
h1.onmouseenter = func;
// Using the old way of adding events overrides all prior events
h1.onclick = func;
```

## IMP BUBBLING AND EVENT PROPAGATION

![[Images-attachments/Untitled 8 3.png|Untitled 8 3.png]]

![[Images-attachments/Untitled 9 2.png|Untitled 9 2.png]]

![[Images-attachments/Untitled 10 2.png|Untitled 10 2.png]]

Remember that the capturing and bubbling phase happens only to the element and its parents and not to its sibling elements.

In the context of JavaScript event propagation, the terms "capturing phase" and "bubbling phase" refer to two different phases of how events are processed and propagated through the DOM (Document Object Model) hierarchy.

1. Capturing Phase:  
    The capturing phase is the first phase in the event propagation process. During this phase, the event is said to be in the "capture" mode. In this mode, the event starts from the outermost ancestor element and traverses down the DOM hierarchy towards the target element that triggered the event. This means that during the capturing phase, the event handlers attached to the ancestor elements are executed before reaching the target element.  
    
2. Bubbling Phase:  
    The bubbling phase is the second phase in the event propagation process, following the capturing phase. After the capturing phase is completed, the event enters the "bubble" mode. In this mode, the event starts from the target element and propagates up the DOM hierarchy towards the outermost ancestor element. During the bubbling phase, the event handlers attached to the target element and its ancestor elements are executed, with the event reaching the outermost ancestor last.  
    

To summarize, in JavaScript event propagation, the capturing phase involves traversing down the DOM hierarchy from the outermost ancestor to the target element, while the bubbling phase involves traversing up the DOM hierarchy from the target element to the outermost ancestor. The capturing and bubbling phases allow events to be handled at different levels of the DOM, providing flexibility and control over event handling in complex HTML structures.

Remember that the capturing and bubbling phase happens only to the element and its parents and not to its sibling elements.

## Event Propagation in Practice and also stopping the propagation //Very IMP Even though it is length study it.

Suppose there are Grand Parents , Parent and child and all three of them have Event Listeners There is definitely no problem when i click on Only Grandparent since only Listener associated to Grand parent is Executed the Problem occurs when parent and child element is clicked because they are subsets of a greater parent element so the dilemma is what should be the order of execution of the events in the code.

When you have event listeners attached to the grandparent, parent, and child elements, the order of event execution depends on the event propagation phases: capturing and bubbling.

1. Capturing Phase:
    - If you click on the child element, the event starts from the highest ancestor (grandparent) and propagates down to the child element in the capturing phase.
    - During the capturing phase, the event listeners attached to the grandparent and parent elements will be executed in that order.
    - So, the order of execution will be: "Grandparent (Capturing)" -> "Parent (Capturing)".
2. Target Phase:
    - After the capturing phase, the event reaches the target element (child) itself, and the event listener associated with the child element executes.
3. Bubbling Phase:
    - Once the target phase is complete, the event starts bubbling up from the child element to the parent and grandparent elements.
    - During the bubbling phase, the event listener attached to the parent element will execute first, followed by the listener attached to the grandparent element.
    - So, the order of execution will be: "Parent (Bubbling)" -> "GrandParent (Bubbling)".

In summary, when you click on the child element, the event listeners are executed in the following order:  
"GrandParent (Capturing)" -> "Parent (Capturing)" -> "Child" -> "Parent (Bubbling)" -> "GrandParent (Bubbling)".  

This order ensures that the event propagates from the highest ancestor down to the target element during the capturing phase and then back up to the highest ancestor during the bubbling phase.

All of this happens when we click the common area of Parent and child divs which maens

```Plain
<!DOCTYPE html>
<head>
  <title>Tarun  </title>

  <style>
  div {
    min-width: 100px;
    min-height: 100px;
    padding: 30px;
    border: 1px solid black;
    background-color: white;
  }
  </style>
</head>
<body>

  <div id="grandparent">
    <div id="parent">
      <div id="child"></div>
    </div>
  </div>

  <script src="test.js"></script>
</body>
</html>
```

In this HTML code, there are three nested `div` elements: `grandparent`, `parent`, and `child`. Each div has its own ID for identification purposes.

In the JavaScript code (`test.js`), event listeners are attached to each of these elements:

```Plain
const grandParent = document.getElementById('grandparent');
const parent = document.getElementById('parent');
const child = document.getElementById('child');

grandParent.addEventListener('click', function(e){
  this.style.backgroundColor='red';
  console.log('GrandParent');
});

parent.addEventListener('click', function(e){
  this.style.backgroundColor='blue';
  console.log('parent');
});

child.addEventListener('click', function(e){
  this.style.backgroundColor='green';
  console.log('child');
});
```

1. Bubbling Phase:  
    When you click on the  
    `child` element, the event is triggered and enters the bubbling phase. It starts propagating upward through the DOM hierarchy from the target element (`child`) to its ancestor elements (`parent` and `grandParent`). In this phase, the event listeners attached to each of these elements will be executed.

So, in this case, the console will log:

```Plain
child
parent
GrandParent
```

The background color of the `child` element will turn green, followed by the `parent` element turning blue, and finally, the `grandParent` element turning red.

1. Capturing Phase:  
    By default, event listeners are attached in the bubbling phase. However, if you want to switch to the capturing phase, you can do so by passing  
    `true` as the third parameter to the `addEventListener` method.

For example, modifying the event listeners as follows:

```Plain
grandParent.addEventListener('click', function(e){
  this.style.backgroundColor='red';
  console.log('GrandParent');
}, true);

parent.addEventListener('click', function(e){
  this.style.backgroundColor='blue';
  console.log('parent');
}, true);

child.addEventListener('click', function(e){
  this.style.backgroundColor='green';
  console.log('child');
}, true);
```

Now, when you click on the `child` element, the event enters the capturing phase. It starts from the outermost ancestor (`grandParent`) and propagates downward through the DOM hierarchy to the target element (`child`).

In this case, the console will log:

```Plain
GrandParent
parent
child
```

The background color of the `grandParent` element will turn red, followed by the `parent` element turning blue, and finally, the `child` element turning green.

Remember that capturing and bubbling phases are optional and can be controlled by passing `true` or `false` as the third parameter to the `addEventListener` method. By default, the event listeners work in the bubbling phase.

**Tricky Cases**

Certainly! Let's modify the event listeners to have a combination of capturing and bubbling phases for the parent and grandparent elements:

```Plain
const grandParent = document.getElementById('grandparent');
const parent = document.getElementById('parent');
const child = document.getElementById('child');

grandParent.addEventListener('click', function(e){
  this.style.backgroundColor='red';
  console.log('GrandParent (Capturing)');
}, true);

parent.addEventListener('click', function(e){
  this.style.backgroundColor='blue';
  console.log('Parent (Bubbling)');
});

child.addEventListener('click', function(e){
  this.style.backgroundColor='green';
  console.log('Child (Capturing)');
}, true);
```

In this updated code, the event listener for the `grandParent` element has the third parameter set to `true`, indicating that it listens in the capturing phase. The event listener for the `parent` element does not specify the third parameter, using the default bubbling phase. The event listener for the `child` element also has the third parameter set to `true`, indicating capturing phase.

When you click on the `child` element, the event propagation will follow these phases:

1. Capturing Phase:
    - The event starts from the outermost ancestor (`grandParent`).
    - The event listener attached to the `grandParent` element executes in the capturing phase, logging "GrandParent (Capturing)" to the console.
    - The event continues to propagate downward to the `parent` element, but since it does not have a capturing phase listener, nothing is logged.
    - The event reaches the target element (`child`).
    - The event listener attached to the `child` element executes in the capturing phase, logging "Child (Capturing)" to the console.
    - The background color of the `child` element turns green.
2. Target Phase:
    - The event is already at the target element (`child`), so no additional listeners execute at this phase.
3. Bubbling Phase:
    - The event starts to propagate upward from the `child` element.
    - The event listener attached to the `parent` element executes in the bubbling phase, logging "Parent (Bubbling)" to the console.
    - The background color of the `parent` element turns blue.
    - The event continues to propagate upward to the `grandParent` element, but since it does not have a bubbling phase listener, nothing is logged.

Therefore, the console will display the following output:

```Plain
GrandParent (Capturing)
Child (Capturing)
Parent (Bubbling)
```

The background color of the `grandParent` element will turn red, the `child` element will turn green, and the `parent` element will turn blue.

Try out More cases to Understand it better and before the interview.

**Stopping the Propagation**

```JavaScript
const grandParent = document.getElementById('grandparent');
const parent = document.getElementById('parent');
const child = document.getElementById('child');

grandParent.addEventListener('click', function(e){
  console.log('GrandParent (Bubbling )');
}, );

parent.addEventListener('click', function(e){
  
  e.stopPropagation(); // Stop event propagation
console.log('Parent (Bubbling)');
});

child.addEventListener('click', function(e){
  console.log('Child (Bubbling)');
});
```

In this case, the event listeners are set to the bubbling phase, and the `stopPropagation()` method is called in the event listener attached to the parent element. Additionally, the console.log statement for the parent element is placed after the `stopPropagation()` call.

When you click on the child element:

1. Bubbling Phase:
    - The event starts at the target element (`child`) and begins to propagate upward to its parent elements.
    - The event listener attached to the `child` element executes, logging "Child (Bubbling)" to the console.
    - The event continues to propagate upward to the parent element.
    - The `stopPropagation()` method is called in the event listener attached to the `parent` element, which stops the event propagation beyond that point.
    - The console.log statement for the parent element, which is placed after `stopPropagation()`, is not executed.
    - The event does not reach the grandparent element.

As a result, the console will only display:

```Plain
Child (Bubbling)
```

The event propagation is stopped at the parent element due to `stopPropagation()`. However, since the console.log statement for the parent element comes after `stopPropagation()`, it is not executed. Only the event listener attached to the child element executes and logs "Child (Bubbling)" to the console.

**remember if stop propagation is before Console.log then Console.Log will get executed**

## Event Delegation //Very Imp to implement while coding Checkout jonas GitHub and Advanced Dom Bankist section code to get various strategies that are useful in real world

Based on Event Bubbling

Sure! Here are some notes on event delegation:

1. Event delegation is a technique in JavaScript where instead of attaching an event listener to individual elements, you attach a single event listener to a parent element. This parent element then handles the events that occur on its child elements.
2. Event delegation is useful when you have multiple elements that share a common parent and you want to handle events on all of them in a similar way.
3. The main advantage of event delegation is that it reduces the number of event listeners in your code. Instead of attaching an event listener to each individual element, you attach one listener to the parent element. This can improve performance and reduce memory consumption, especially when dealing with a large number of elements.
4. Event delegation works by utilizing event bubbling, where an event occurring on a child element will propagate up the DOM hierarchy to its parent elements.
5. When using event delegation, the event object that is passed to the event listener will still reference the child element that triggered the event. You can access this element using properties like `event.target` or `event.currentTarget` within the event listener.
6. Event delegation allows you to dynamically add or remove child elements without needing to attach or detach event listeners to them individually. As long as the parent element remains the same, the event handling will still work for the new or removed child elements.
7. However, be cautious when using event delegation with events that have specific behavior based on the target element. In such cases, you may need to check the target element within the event listener to ensure the desired behavior is applied appropriately.

Overall, event delegation is a powerful technique for handling events efficiently and simplifying event management in your JavaScript code. By attaching event listeners to parent elements and utilizing event bubbling, you can handle events on multiple elements with a single event listener.

**Simply Summarised instead of creating multiple events for children Create an Event for the parent then Due to the concept of bubbling the parent event is triggered even though child is clicked**

```JavaScript
/* HTML
<ul id="parent">
  <li class="child">One</li>
  <li class="child">Two</li>
  <li class="child">Three</li>
  <li class="child">Four</li>
  <li class="child">Five</li>
</ul>;
*/

// Obvious approach
const children = Array.from(document.getElementsByClassName("child"));

children.forEach((child) => {
  child.addEventListener("click", () => {
    /* Do Something */
  });
});

const newLi = document.createElement("li");
newLi.textContent = "Six";
// A new event listener must be added to each new element (!DRY)
newLi.addEventListener("click", () => {
  /* Do Something */
});
parent.append(newLi);


// Event Delegation
const parent = document.getElementById("parent");

parent.addEventListener("click", (event) => {
  console.log(e.target); // Identify where the event happened

  if (event.target.matches("li")) { // Check if the element is li
    /* Do Something */
  }
});

// There is no need to add new event listeners to new elements with Event Delegation
const newLi = document.createElement("li");
newLi.textContent = "Six";
parent.append(newLi);


/* Making the event delegation work requires 4 steps:
1. Determine the parent of elements to watch for events
2. Attach the event listener to the parent element
3. Use event.target to select the target elements
4. Check if the target is the desired element */
```

  

## DOM Traversing

```JavaScript
/* HTML
<header class="header">
  <div class="header__title">
    <h1>
      <span class="highlight">banking</span>
      <span class="highlight">minimalist</span>
    </h1>
    <h4>A simpler banking experience for a simpler life.</h4>
    <button class="btn">Learn more</button>
    <img
      src="img/hero.png"
      class="header__img"
      alt="Minimalist bank items"
    />
  </div>
</header>
*/

const h1 = document.querySelector("h1");

// Going downwards: child
// Selects all class with highlight within h1 (children)
console.log(h1.querySelectorAll(".highlight"));
console.log(h1.childNodes);
console.log(h1.children);
h1.firstElementChild.style.color = "white";
h1.lastElementChild.style.color = "orange";

// Going upwards: parents
console.log(h1.parentNode);
console.log(h1.parentElement);
//.closest very imp
h1.closest(".header").style.background = "var(--gradient-secondary)";//You can say that this is opposite to the queryselector because it selects the parents where as query selector selects the children.

h1.closest("h1").style.background = "var(--gradient-primary)";

// Going sideways: siblings
console.log(h1.previousElementSibling);
console.log(h1.nextElementSibling);

console.log(h1.previousSibling);
console.log(h1.nextSibling);

console.log(h1.parentElement.children); // Get all siblings
[...h1.parentElement.children].forEach(function (el) {
  if (el !== h1) el.style.transform = "scale(0.5)";
});
```

## Building a Tabbed Component

  

## Intersection Observer API//very useful for animations while scrolling and many more

The Observer API, also known as the Intersection Observer API, is a web API that provides a way to asynchronously observe changes in the intersection between elements and the viewport or other elements. It allows you to efficiently detect and track when an observed element enters or exits the viewport or intersects with another element.

The Intersection Observer API is particularly useful for implementing features such as lazy loading of images, infinite scrolling, or triggering animations based on element visibility.

The main components of the Intersection Observer API are:

1. **Intersection Observer**: This is the main object that observes the target elements and triggers a callback when the intersection changes.
2. **Target Element**: The element that you want to observe and track its intersection with another element or the viewport.
3. **Callback**: A function that gets executed when the intersection between the target element and the root element (viewport or another element) changes. It receives an array of `IntersectionObserverEntry` objects as an argument, providing information about the intersection.
4. **IntersectionObserverEntry**: An object that represents the observed element and provides information about its intersection with the root element. It contains properties like `isIntersecting`, `intersectionRatio`, and `target`.

Here's a basic example of using the Intersection Observer API to observe the visibility of an element:

```Plain
// Create an observer instance
const observer = new IntersectionObserver((entries) => {
  entries.forEach((entry) => {
    if (entry.isIntersecting) {
      // Element is visible
      console.log('Element is visible');
    } else {
      // Element is not visible
      console.log('Element is not visible');
    }
  });
});

// Select the target element to observe
const targetElement = document.querySelector('.target-element');

// Start observing the target element
observer.observe(targetElement);
```

In this example, the observer is created using the `IntersectionObserver` constructor, and a callback function is provided. Whenever the visibility of the target element (`.target-element`) changes, the callback function is called with the corresponding `IntersectionObserverEntry` object. The `isIntersecting` property of the entry indicates whether the element is currently intersecting with the root element.

You can configure additional options for the observer, such as setting the `root` element, `threshold`, and using the `rootMargin` to adjust the intersection area.

The Intersection Observer API provides a powerful and efficient way to handle visibility-related events without relying on scroll events or manual calculations, improving performance and user experience.

## DOM lifecycle

```JavaScript
/* The DOMContentLoaded event fires when the initial HTML document has 
been completely loaded and parsed, without waiting for stylesheets and 
images to finish loading. Only handle DOMContentLoaded event if you 
placed the script in the head without defer or async. */
document.addEventListener("DOMContentLoaded", function (e) {
  console.log("HTML parsed and DOM tree built!", e);
});

// This is equivalent to DOMContentLoaded in Vanilla JS (jQuery).
$(document).ready();

/* The load event is fired when the whole page has loaded, including all 
dependent resources such as stylesheets and images. */
window.addEventListener("load", function (e) {
  console.log("Page fully loaded", e);
});

/* The beforeunload event is fired just before the user is about to leave 
a page. It can be used to ask users if they are 100% sure they want to 
leave the page. */
window.addEventListener("beforeunload", function (e) {
  // Some browsers require us to call preventDefault() in order to use this
  e.preventDefault();
  console.log(e);
  // returnValue displays warning before leaving the page (!It's been deprecated)
  e.returnValue = "";
});
```

## Defer and Async Script Loading //IMP

Defer way is the most preferred way to load the scripts

  

![[Images-attachments/Untitled 11 2.png|Untitled 11 2.png]]

![[Images-attachments/Untitled 12 2.png|Untitled 12 2.png]]

![[Images-attachments/Untitled 13 2.png|Untitled 13 2.png]]