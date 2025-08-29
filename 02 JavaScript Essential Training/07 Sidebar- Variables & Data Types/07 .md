# Stricter Typing in JavaScript

JavaScript is a weakly typed language, meaning variables can hold values of any type without declaring what type of data they hold.

This can produce unexpected behaviours if we're not careful about the data types we use.

Consider this basic addition function:

```js
function add(a, b) {
 return a + b;
}

console.log(add(5, 10)); // Output: 15
console.log(add("5", 10)); // Output: "510"
```

Without strong typing, we can't tell the function to only accept numbers as arguments. As a result, if we accidentally pass in a string instead of a number, the function will concatenate the values together instead of adding them, and suddenly "5" + "10" becomes "510."

This stands in sharp contrast to many other languages including Java, C++, and Python which are more strongly typed. In strongly typed languages, you declare the type of a variable when you define it, and if the data type does not match the declaration, a warning or error is triggered. This can help prevent errors and bugs in code which is why strongly typed languages are often preferred for large applications.

But as I explained, JavaScript is a weakly typed language, yet many of the biggest applications in the world are built using JavaScript. Does that mean those applications are less secure?

There are several connected answers here:

First, to introduce strong typing to JavaScript applications, many developers use TypeScript, a strongly typed variant of JavaScript that adds static typing to solve this exact problem (thus the name). TypeScript is then compiled into JavaScript before being run in the browser.

Second, we can mimic stronger typing in JavaScript using some basic strategies. Let's break them down.

First, we can use the typeof operator to identify the data type of any value, and then test to see if the data type matches what is expected. You manually verify data types at runtime to ensure they match expectations.

Implementing this strategy for our add function would look like this:

```js
function add(a, b) {
 if (typeof a !== "number" || typeof b !== "number") {
   throw new TypeError("Both arguments must be numbers");
 }
 return a + b;
}
```

This approach could be expanded further to identify the type of each argument and tell the developer what data is being passed in. It can also be expanded to include more complex data types like objects or arrays.

We can expand on this approach and make it DRY by creating a generic assert function that throws an error when data does not meet a specified type. This function can then be reused any time we need type checking.

Here's what the add function would look like using an assert function:

```js
function assertType(value, type) {
 if (typeof value !== type) {
   throw new TypeError(`Expected ${type}, but got ${typeof value}`);
 }
}

function add(a, b) {
 assertType(a, "number");
 assertType(b, "number");
 return a + b;
}
```

For larger applications, this DRY approach is quicker to implement and easier to understand and maintain.

For more complex data types, we can integrate type checks directly into constructor functions or classes to ensure objects are created with valid types.

The example below shows the familiar Backpack class with type checks added to the constructor using an expanded assertType() function:

```js
function assertType(value, type, paramName) {
 if (typeof value !== type) {
   throw new TypeError(`${paramName} must be a ${type}`);
 }
}

class Backpack {
 constructor(
   name,
   volume,
   color,
   pocketNum,
   strapLengthL,
   strapLengthR,
   lidOpen
 ) {
   assertType(name, "string", "name");
   assertType(volume, "number", "volume");
   assertType(color, "string", "color");
   assertType(pocketNum, "number", "pocketNum");
   assertType(strapLengthL, "number", "strapLengthL");
   assertType(strapLengthR, "number", "strapLengthR");
   assertType(lidOpen, "boolean", "lidOpen");

   this.name = name;
   this.volume = volume;
   this.color = color;
   this.pocketNum = pocketNum;
   this.strapLength = {
     left: strapLengthL,
     right: strapLengthR,
   };
   this.lidOpen = lidOpen;
 }

 toggleLid(lidStatus) {
   assertType(lidStatus, "boolean", "lidStatus");
   this.lidOpen = lidStatus;
 }

 newStrapLength(lengthLeft, lengthRight) {
   assertType(lengthLeft, "number", "lengthLeft");
   assertType(lengthRight, "number", "lengthRight");
   this.strapLength.left = lengthLeft;
   this.strapLength.right = lengthRight;
 }
}
```

This last example hints at why type checking is so important. When we start working with larger functions and complex data—especially if that data comes from an outside source like an API—it is vital to ensure the data going into different functionality is the type we expect it to be.

The techniques showcased here give you more control over data types in your code, and for many applications they will be sufficient. However, if you find yourself writing a lot of type checking code, it might be time to switch your project to TypeScript. That way, you get all the goodness of JavaScript with strong typing built right in!