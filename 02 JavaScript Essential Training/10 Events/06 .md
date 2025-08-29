# This: A Deeper Dive

The this keyword in JavaScript has far broader uses than what its name and general examples suggest.

We encountered this when we discussed object constructors and classes. Here's our Backpack class for reference:

```js
class Backpack {
 constructor(
   // Defines parameters:
   name,
   volume,
   color,
   pocketNum,
   strapLengthL,
   strapLengthR,
   lidOpen
 ) {
   // Define properties:
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
 // Add methods like normal functions:
 toggleLid(lidStatus) {
   this.lidOpen = lidStatus;
 }
 newStrapLength(lengthLeft, lengthRight) {
   this.strapLength.left = lengthLeft;
   this.strapLength.right = lengthRight;
 }
}
```

Here, this is used to refer to the object the method is called on, so when the class is used to create a new object the code literally says "this object's name property is equal to the value of the name argument". This is a common use of this in JavaScript, but it's not the only one.

To simplify code, JavaScript allows you to chain multiple methods together when working with data. In the below example, the concat(), toUpperCase(), and trim() methods are chained together to modify a string:

```js
let greeting = "Hello";

greeting = greeting.concat(" World").toUpperCase().trim();

console.log(greeting); // Outputs: "HELLO WORLD"
```

Using the this keyword, you can enable method chaining in your own classes.

Here’s the Backpack class with two small modifications:

```js
class Backpack {
 constructor(
   // Defines parameters:
   name,
   volume,
   color,
   pocketNum,
   strapLengthL,
   strapLengthR,
   lidOpen
 ) {
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
   this.lidOpen = lidStatus;
   return this; // Return the current object to enable chaining
 }

 newStrapLength(lengthLeft, lengthRight) {
   this.strapLength.left = lengthLeft;
   this.strapLength.right = lengthRight;
   return this; // Return the current object to enable chaining
 }
}

// Usage
const myBackpack = new Backpack("Hiker", 30, "blue", 5, 35, 35, false);
myBackpack.toggleLid(true).newStrapLength(25, 25);
```

Now both toggleLid and newStrapLength use this to return the instance after performing their actions, allowing for the methods to be called in a continuous chain.

The example above barely scratches the surface of what this can do, and also illustrates how quickly JavaScript can become complex. A good rule of thumb is to remember this always refers to the object the method is called on. And when you're ready to dive deeper (or this doesn't work as expected), refer back to the official documentation for help.