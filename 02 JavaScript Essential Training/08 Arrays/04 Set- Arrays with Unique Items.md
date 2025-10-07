# Set: Arrays with Unique Items

The backpack featured in this course is the backpack I use any time I go on a trip and need to bring my laptop and other equipment.

Every time our family travels, my wife and I both make lists: arrays of items we need to pack. Those arrays ensure we don't forget anything, but if we're not careful, we end up adding the same item multiple times. To avoid this, we could combine the arrays into one, and then manually remove any duplicates. Or, we could use Set.

The Set object in JavaScript stores unique values of any type, with an emphasis on unique. You can't have the same value more than once in a Set. This makes it perfect for storing a collection of unique items, like the items in our packing list. Unlike an array where items are ordered and you access them by their index number, the items in a Set are unordered and cannot be accessed randomly.

Here's what our packing list looks like as a Set:

```js
const itemsToPack = new Set();
itemsToPack.add("water bottle");
itemsToPack.add("laptop");
itemsToPack.add("headphones");
itemsToPack.add("snacks");
itemsToPack.add("book");
itemsToPack.add("sunscreen");
```

The Set is created using the new Set() constructor. We then add items to the Set using the add() method. The Set object has several methods to help you manage the collection of items:

- add(): Adds a new item to the Set

- has(): Checks if an item exists in the Set

- delete(): Removes an item from the Set

- size(): Returns the number of items in the Set

Once a Set has been created, we can use the above methods to modify it. For example, we can check if an item is in the Set and add it if it's not:

```js
if (!itemsToPack.has("water bottle")) {
 itemsToPack.add("water bottle");
} else {
 console.log("Water bottle is already on the list!");
}
```

## Accessing specific items within a Set 

Set is often referred to as arrays with unique items (as in the title of this article), but this mental model may lead you astray until you get a full grasp on when to use a Set over an array. Specifically, one of the first things you probably want to know is how to access specific items within a Set.

When you feel this urge, ask yourself: Why do I need access to that specific item beyond knowing it's there (for which there is the has() method described above)? Chances are, if you're asking that question, you shouldn't be using a Set:

- If you need to access items by index, you should probably be using an array

- If you need to access items by key, you should probably be using an Object

- If you need to access items by value, you should probably be using a Map

Set is specifically for when you need to store a collection of unique items and then perform operations on that collection.

So what are the uses of Set then? Let's return to our packing list example. Once we have a list of all the items we need to pack, we can use the forEach() method to loop through the items and check to make sure they're actually packed:

```js
const backpack1.items = ["water bottle", "laptop"];
const backpack2.items = [ "headphones", "snacks", "book"];

itemsToPack.forEach(item => {
   if (backpack1.items.includes(item)) {
       console.log(`${item} is packed in backpack 1`);
   } else if (backpack2.items.includes(item)) {
       console.log(`${item} is packed in backpack 2`);
   } else {
       console.log(`${item} is not packed`);
   }
});
```

Run this code and you'll see which backpack contains which item from the list (and that we once again forgot to pack the sunscreen).

Set is one of those features you don't see a use for until you suddenly need it. It's a powerful tool for managing collections of unique items, and provides a quick and straightforward way of ensuring you don't have duplicates in your data, and removing them if you do.