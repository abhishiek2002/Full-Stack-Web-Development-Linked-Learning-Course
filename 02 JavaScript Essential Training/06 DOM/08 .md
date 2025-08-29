# Working with data-* Attributes

The cool thing about HTML elements is you can add as many attributes as you like to any one of them, and then use JavaScript to create, read, update, modify, and delete them at will. It's useful to think of these attributes as feature lists for the element. Each attribute can change the element's behavior or appearance or even relationship with other elements.

As we've already covered, for a standard HTML element, its attributes can be manipulated using methods like getAttribute(), setAttribute(), and removeAttribute(). These methods allow you to interact directly with the element's attributes by dynamically editing the HTML source code without reloading the page.

Given the following HTML element...

```js
<button id="myButton" class="btn btn-primary" type="button">Click Me!</button>
```
...you can manipulate its attributes like this:

```js
const button = document.querySelector("#myButton");

// Get the value of the 'class' attribute
console.log(button.getAttribute("class")); // "btn btn-primary"

// Overwrite the value of the 'class' attribute
button.setAttribute("class", "btn btn-secondary");

// Remove the 'type' attribute
button.removeAttribute("type");
```

These manipulations serve as straightforward examples of a much broader and more powerful concept. In addition to describing the element by adding basic attributes like classes, attributes can be used to store custom data and even define roles for accessibility. One common use of this feature is to create custom data attributes with a data-* prefix and retrieve them using the dataset property.  

As an example, in the below example a custom data-sku attribute is appended to each available product, and the SKUs of the selected products is passed on when the submit button is pressed:

```js
<form id="productForm" onsubmit="submitProducts(event)">
  <fieldset>
    <legend>Select products:</legend>

    <label for="product1">
      <input type="checkbox" id="product1" class="product" data-sku="SKU1234" />
      Product 1
    </label>

    <label for="product2">
      <input type="checkbox" id="product2" class="product" data-sku="SKU5678" />
      Product 2
    </label>

    <label for="product3">
      <input
        type="checkbox"
        id="product3"
        class="product"
        data-sku="SKU91011"
      />
      Product 3
    </label>

    <button type="submit">Submit Selection</button>
  </fieldset>
</form>
<form id="productForm" onsubmit="submitProducts(event)">
  <fieldset>
    <legend>Select products:</legend>

    <label for="product1">
      <input type="checkbox" id="product1" class="product" data-sku="SKU1234" />
      Product 1
    </label>

    <label for="product2">
      <input type="checkbox" id="product2" class="product" data-sku="SKU5678" />
      Product 2
    </label>

    <label for="product3">
      <input
        type="checkbox"
        id="product3"
        class="product"
        data-sku="SKU91011"
      />
      Product 3
    </label>

    <button type="submit">Submit Selection</button>
  </fieldset>
</form>
```

Custom data attributes are frequently used in JavaScript frameworks and libraries to store additional information about an element. They are also used to define roles for accessibility, such as aria-* attributes. These attributes are used to provide additional information to assistive technologies like screen readers, making the web more accessible to users with disabilities.