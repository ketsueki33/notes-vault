Event handling in the DOM (Document Object Model) is **the process of detecting and responding to user interactions or system events on a web page**. Events can be triggered by a variety of actions, such as clicking a button, submitting a form, scrolling the page, or resizing the window. In the DOM, events are represented as objects, and event handling is implemented through event listeners.

#### Event Listeners
Event listeners are the foundation of event handling in the DOM. An event listener is a function that waits for a specific event to occur on an HTML element and executes a set of instructions when the event is triggered. The event listener is attached to the HTML element using the `addEventListener()` method, which takes two arguments: the name of the event to listen for and the function to be executed when the event is triggered.
```js
const button = document.querySelector('button');

button.addEventListener('click', () => {
  alert('Button clicked!');
});
```

#### 'this' inside event handlers
The [['this' keyword#Event Handler Context|this]] keyword in JavaScript event handlers refers to the element that received the event. So we can write separate functions that will work with the function `addEventListener()`.
```js
const button = document.getElementById('myButton');
button.addEventListener('click', function() {
console.log(this); // <button> element
```

Note that, `this` will not behave like this inside Arrow functions as they inherit `this` from the enclosing lexical context.

#### Event Objects
Event objects are objects that are automatically passed in to our callbacks inside an event handler. We can capture it by giving an argument.

We have to use event objects with keyboard events to find out what key was pressed.
```js
const input = document.querySelector("input");
input.addEventListener("keydown", function (e) {
    console.log(e.key);
    console.log(e.code);
});
```

##### Useful Event Object Methods
###### preventDefault( )
The `event.preventDefault()` method is used to prevent the default action that belongs to the event from occurring. For example, if you have a form submission event, calling `event.preventDefault()` on that event will prevent the form from submitting.
```js
document.querySelector('form').addEventListener('submit', function(event) {
  event.preventDefault(); // Prevents the form from submitting
  console.log('Form submission prevented');
});
```

###### stopPropagation()
Events will bubble up i.e clicking on an element will also trigger the click event of the parent’s click event listener and also the grandparent’s click event listener. The `event.stopPropagation()` method is used to stop the event from bubbling up the event chain. In other words, it prevents the event from propagating to parent elements.
```js
document.querySelector('.child').addEventListener('click', function(event) {
  event.stopPropagation(); // Prevents the event from bubbling up to parent elements
  console.log('Child element clicked');
});

document.querySelector('.parent').addEventListener('click', function() {
  console.log('Parent element clicked');
});
```

#### Mouse Events

| event        | description                                                                                                                                      |
| ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------ |
| `click`<br>  | Fired when a pointing device button (e.g., a mouse's primary button) is pressed and released on a single element.                                |
| `auxclick`   | Fired when a non-primary pointing device button (e.g., any mouse button other than the left button) has been pressed and released on an element. |
| `dblclick`   | Fired when a pointing device button (e.g., a mouse's primary button) is clicked twice on a single element.                                       |
| `mousedown`  | Fired when a pointing device button is pressed on an element.                                                                                    |
| `mouseup`    | Fired when a pointing device button is released on an element.                                                                                   |
| `mouseenter` | Fired when a pointing device (usually a mouse) is moved over the element that has the listener attached.                                         |
| `mouseleave` | Fired when the pointer of a pointing device (usually a mouse) is moved out of an element that has the listener attached to it.                   |
| `mouseover`  | Fired when a pointing device is moved onto the element to which the listener is attached or onto one of its children.                            |

**Example)** A button that changes text color of a paragraph.
```js
const para = document.querySelector(".paragraph");
const btn = document.querySelector(".btn");

btn.addEventListener("click", () => {
    if (para.style.color === "red") para.style.color = "blue";
    else para.style.color = "red";
});
```

**Example)** An image that toggles its visibility on double click.
```js
const image = document.querySelector(".image");
image.addEventListener("dblclick", () => {
    image.classList.toggle("hidden");
});
```

#### Keyboard Events
| event     | description                   |
| --------- | ----------------------------- |
| `keydown` | Fired when a key is pressed.  |
| `keyup`   | Fired when a key is released. |
**Example)** `keydown` event listener in an input field that logs the key pressed to console.
```js
const input = document.querySelector(".text-input");
input.addEventListener("keydown", function (e) {
    console.log(e.key);
    console.log(e.code);
});
// Output if 'S' is pressed
// s
// KeyS
```

**Example)** `keyup` event listener in an input field that displays the current value in a paragraph.
```js
const paragraph = document.querySelector(".paragraph");

const input = document.querySelector(".text-input");
input.addEventListener("keyup", function (e) {
    paragraph.textContent = this.value;
});
```

#### Form Events
| event    | description                                                                                                                                                                                                                                     |
| -------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `submit` | The **`submit`** event fires when a `<form>` is submitted.                                                                                                                                                                                      |
| `change` | The `change` event is fired for `<input>`, `<select>`, and `<textarea>` elements when the user modifies the element's value. Unlike the `input` event, the `change` event is not necessarily fired for each alteration to an element's `value`. |
| `input`  | The input event fires when the value of an `<input>`, `<select>`, or `<textarea>` element has been changed as a direct result of a user action                                                                                                  |
**Example)** `sumbit` event listener to a form that logs form data to console.
```js
const form = document.querySelector("form");

form.addEventListener("submit", function (e) {
    e.preventDefault();
    const formData = new FormData(form);
    console.log(formData);
    for (let [name, value] of formData.entries()) {
        console.log(`${name}: ${value}`);
    }
});
```

**Example)** Add a change event listener to a select dropdown that displays the selected value in a paragraph
```js
const paragraph = document.querySelector(".paragraph");
const select = document.querySelector(".select");

select.addEventListener("change", (e) => {
    paragraph.textContent = e.target.value;
});
```

#### Event Delegation
Event delegation leverages the concept of event propagation (bubbling) to handle events efficiently. Instead of attaching event listeners to multiple child elements, you attach a single event listener to a parent element. This listener can then handle events from all child elements, even if those elements are added dynamically.

**How it works?**
When an event occurs on an element, it doesn't just trigger the event handler for that element; it also triggers handlers on its ancestors, all the way up to the root of the document. This process is called event bubbling.

Event delegation utilizes this by setting up an event listener on a common ancestor of all the elements you want to handle events for. The event listener can then determine which specific element triggered the event by checking the event object’s `target` property.

**Example)** Add a click event listener to a list that logs the text content of the clicked list item using event delegation.
```js
const list = document.querySelector(".list");
list.addEventListener("click", function (e) {
    if (e.target && e.target.nodeName === "LI") {
        console.log(e.target.textContent);
    }
});
```
