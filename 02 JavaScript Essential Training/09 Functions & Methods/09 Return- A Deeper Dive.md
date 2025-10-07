# Return: A Deeper Dive

The return keyword in JavaScript is far more versatile than both its name and general examples suggest. So let's dive a little bit deeper and see how simply returning from a function can open up a large possibility space.

For reference, here's the standard use of return in a function:

```js
function sum(a, b) {
 let result = a + b;
 return result;
}
console.log(sum(5, 3)); // Outputs: 8
```

Here, return is doing its primary job: literally returning a value to the caller.

## Early exit

Hidden by the standard example above is how return terminates the execution of any further parts of the function. This feature is crucial for controlling when complex functions run and for how long they run.

Consider this example where return is used to stop a function early:

```js
function calculateDiscount(items) {
 // Check if there is more than one item
 if (items.length <= 1) {
   console.log("Discount is only applied when there are more than one item.");
   return; // Exits the function if not more than one item
 }

 // Calculate total price of all items
 let totalPrice = items.reduce((sum, item) => sum + item.price, 0);

 // Calculate 15% discount
 let discount = totalPrice * 0.15;

 // Calculate final price after discount
 let discountedPrice = totalPrice - discount;

 // Return the discounted price
 return discountedPrice;
}

// Example usage:
const items = [
 { name: "Book", price: 20 },
 { name: "Pen", price: 5 },
];

const discountedPrice = calculateDiscount(items);
console.log("Discounted price: $" + discountedPrice.toFixed(2));
```

Here the return statement is used to exit the function early if the condition is met, preventing the rest of the function from running. For large applications, this approach is essential for speed and efficiency.


## Returning functions

return doesn't just terminate a process and return a value. It can also return functions, also called higher-order functions. This is a powerful feature that can be used to create configurable functions or for currying.

Here's an example of returning a function:

```js
function createMultiplier(multiplier) {
 return function (value) {
   return value * multiplier;
 };
}

const double = createMultiplier(2);
const quintuple = createMultiplier(5);

console.log(double(5)); // Outputs: 10
console.log(quintuple(5)); // Outputs: 25
```

In this example, createMultiplier returns a function that multiplies a value by the multiplier parameter. We then use this returned function to create new functions that multiply by 2 and 5 respectively. Finally, we call these new functions with a value to see the results.

Modern JavaScript applications often rely on asynchronous operations to fetch data from servers through APIs or perform other tasks that take time. In these cases, return is used to handle the asynchronous results.

```js
async function fetchData(url) {
 let response = await fetch(url);
 let data = await response.json();
 return data; // Returns a promise that resolves to data
}
```

Here, return is used to return the data fetched from the server, and once the data is returned, we can output it to the console or use it in other parts of the application.

Inspecting code, you'll notice arrow functions often omit the return keyword altogether. This is because these functions have a special syntax that allows for implicit returns.

```js
const add = (a, b) => a + b; // Implicit return
```

This feature of arrow functions simplifies function declarations significantly when only a single expression is involved.