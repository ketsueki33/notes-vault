The Document Object Model (DOM) is a programming interface for web documents. It represents the page so that programs can change the document structure, style, and content. The DOM represents the document as nodes and objects; that way, programming languages can interact with the page.

A web page is a document that can be either displayed in the browser window or as the HTML source. In both cases, it is the same document but the Document Object Model (DOM) representation allows it to be manipulated. As an object-oriented representation of the web page, it can be modified with a scripting language such as JavaScript.

**DOM and JavaScript**

The DOM is not part of the JavaScript language, but is instead a Web API used to build websites. JavaScript can also be used in other contexts. For example, Node.js runs JavaScript programs on a computer, but provides a different set of APIs, and the DOM API is not a core part of the Node.js runtime.

The DOM was designed to be independent of any particular programming language, making the structural representation of the document available from a single, consistent API. Even if most web developers will only use the DOM through JavaScript, implementations of the DOM can be built for any language.

#### Selecting Elements
###### 1. `getElementById`
This method selects a single element by its `id` attribute. It returns the element if found, otherwise `null`.
```js
const element = document.getElementById('myId');
console.log(element); // <div id="myId">...</div>
```

###### 2. `getElementsByClassName`
This method selects all elements with a specific class name. It returns an HTMLCollection, which is a live collection of elements.
```js
const elements = document.getElementsByClassName('myClass');
console.log(elements); // HTMLCollection of elements with class 'myClass'
```

###### 3. `getElementsByTagName`
This method selects all elements with a specific tag name. It also returns an HTMLCollection.
```js
const elements = document.getElementsByTagName('div');
console.log(elements); // HTMLCollection of <div> elements
```

###### 4. `querySelector`
This method selects the first element that matches a CSS selector. It returns the element if found, otherwise `null`.
```js
const element = document.querySelector('.myClass');
console.log(element); // The first element with class 'myClass'
```

###### 5. `querySelectorAll`
This method selects all elements that match a CSS selector. It returns a NodeList, which is a static collection of elements.
```js
const elements = document.querySelectorAll('.myClass');
console.log(elements); // NodeList of elements with class 'myClass'
```

###### NodeList vs HTMLCollection

|                 | NodeList                                                                                                                                                                | HTMLCollection                                                                       |
| --------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------ |
| **Type**        | General collection of nodes. A node is any individual element in the DOM tree. This could be elements, attributes, texts, comments, and so on.                          | Collection of HTML elements.                                                         |
| **Methods**     | Can use `forEach`, `item`, and `keys`. It also supports array-like indexing.                                                                                            | Can use `item`, `namedItem`, and array-like indexing. Does not have `forEach`.       |
| **Static/Live** | Can be either live or static depending on how it's obtained. For example, `querySelectorAll` returns a static `NodeList`, while `childNodes` returns a live `NodeList`. | Always live. If the document changes, the `HTMLCollection` is automatically updated. |
[more info](https://www.freecodecamp.org/news/dom-manipulation-htmlcollection-vs-nodelist/)

#### Manipulating Elements
##### Changing Content/Style
**innerHTML** :
Allows you to set or get the HTML content of an element (both the HTML markup and the text content of the element).
```js
document.getElementById("myElement").innerHTML = "<p>New content</p>";
```

**innerText** :
This property focuses on the rendered text content. When you use `innerText` to read the content of an element, it returns the text as it appears on screen. It ignores HTML tags. And it also does not include text that is hidden with CSS styles.
```js
document.getElementById("myElement").innerText = "New text content";
```

**textContent** :
The `textContent` property also ignores all HTML tags and returns only the text. Whiles `innerText` reads text as it is rendered on screen, `textContent` reads text as it is in the markup. It also returns all text, whether it's rendered on screen or not.

**style** :
Directly modifies the inline CSS of an element.
```js
document.getElementById("myElement").style.color = "red";
document.getElementById("myElement").style.backgroundColor = "blue";
```

##### Creating and Appending Elements
**createElement** :
creates a new HTML element.
```js
const newElement = document.createElement("div");
```

**appendChild** :
appends the element passed on in the variable as the last child .
```js
const newElement = document.createElement("div");
newElement.textContent = "Appended text content";
document.getElementById("myElement").appendChild(newElement);
```

**insertAdjacentHTML** : 
Inserts HTML at a specified position relative to the element.
```js
document.getElementById("myElement").insertAdjacentHTML('beforeend', '<p>Inserted content</p>');
```
position must be one of the following:

- `"beforebegin"`: Before the element. Only valid if the element is in the DOM tree and has a parent element.

- `"afterbegin"`: Just inside the element, before its first child.

- `"beforeend"`: Just inside the element, after its last child.

- `"afterend"`: After the element. Only valid if the element is in the DOM tree and has a parent element.

**insertAdjacentElement** :
Inserts a given element node at a specified position relative to the target element.  `position` must be one of the strings mentioned above for `insertAdjacentHTML`.
```js
const myElement = document.getElementById("myElement");

// Create new elements
const newElement1 = document.createElement("p");
newElement1.textContent = "Inserted beforebegin";

const newElement2 = document.createElement("p");
newElement2.textContent = "Inserted afterend";

// Insert new elements
myElement.insertAdjacentElement("beforebegin", newElement1);
myElement.insertAdjacentElement("afterend", newElement2);

// Result:
// <p>Inserted beforebegin</p>
// <div id="myElement">Original content</div>
// <p>Inserted afterend</p>
```

**append** :
The `.append()` method inserts a set of `Node` objects or `DOMString` objects after the last child of the target element. This method allows you to insert multiple elements or text nodes at once.
```js
const myElement = document.getElementById("myElement");

// Create new elements
const newElement1 = document.createElement("p");
newElement1.textContent = "Appended paragraph 1";

const newElement2 = document.createElement("p");
newElement2.textContent = "Appended paragraph 2";

// Append new elements
myElement.append(newElement1, newElement2, "Appended text");

// Result: 
// <div id="myElement">
//     Original content
//     <p>Appended paragraph 1</p>
//     <p>Appended paragraph 2</p>
//     Appended text
// </div>
```

**prepend** :
The `.prepend()` method inserts a set of `Node` objects or `DOMString` objects before the first child of the target element. This method allows you to insert multiple elements or text nodes at once.
```js
const myElement = document.getElementById("myElement");

// Create new elements
const newElement1 = document.createElement("p");
newElement1.textContent = "Prepended paragraph 1";

const newElement2 = document.createElement("p");
newElement2.textContent = "Prepended paragraph 2";

// Prepend new elements
myElement.prepend(newElement1, newElement2, "Prepended text");

// Result:
// <div id="myElement">
//     <p>Prepended paragraph 1</p>
//     <p>Prepended paragraph 2</p>
//     Prepended textOriginal content
// </div>
```

**after** :
The `.after()` method inserts a set of `Node` objects or `DOMString` objects immediately after the target element. This method allows you to insert multiple elements or text nodes at once.
```js
const myElement = document.getElementById("myElement");

// Create new elements
const newElement1 = document.createElement("p");
newElement1.textContent = "After paragraph 1";

const newElement2 = document.createElement("p");
newElement2.textContent = "After paragraph 2";

// Insert new elements after myElement
myElement.after(newElement1, newElement2, "After text");

// Result:
// <div id="myElement">Original content</div>
// <p>After paragraph 1</p>
// <p>After paragraph 2</p>
// After text
```

##### Removing Elements

> [!NOTE]+
> If the element has event listeners attached, make sure to remove them to avoid memory leaks.
> 
> This can be done by using `removeEventListener` method:
> 
> ```js
> const element = document.getElementById("elementToRemove");
> 
> function handleClick() {
>     console.log("Clicked!");
> }
> 
> element.addEventListener("click", handleClick);
> element.removeEventListener("click", handleClick);
> element.remove();
> ```

**removeChild** :
You can use the `removeChild()` method to remove a child node from its parent node.
```js
const parent = document.getElementById("parentElement");
const child = document.getElementById("childElement");

parent.removeChild(child);
```

**remove** :
The `remove()` method is a more modern and straightforward way to remove an element from the DOM.
```js
const element = document.getElementById("elementToRemove");

element.remove();
```

**replaceChild** :
You can replace an existing child node with a new node, effectively removing the old one.
```js
const parent = document.getElementById("parentElement");
const oldChild = document.getElementById("oldChildElement");
const newChild = document.createElement("div");

parent.replaceChild(newChild, oldChild);
```

##### Modifying Classes and Attributes
**classname** :
Sets or gets the value of the class attribute.
```js
document.getElementById("myElement").className = "newClass";
```

**classList** :
gets the list of classes. Also provides methods to add, remove and **toggle** CSS classes.
```js
document.getElementById("myElement").classList.add("newClass");
document.getElementById("myElement").classList.remove("oldClass");
document.getElementById("myElement").classList.toggle("toggleClass");
```

**getAttribute** :
returns the value of a specified attribute on the element. If the given attribute does not exist, the value returned will be `null`.
```js
const div1 = document.getElementById("div1");
// <div id="div1">Hi Champ!</div>

const exampleAttr = div1.getAttribute("id");
// "div1"

const align = div1.getAttribute("align");
// null
```

**setAttribute** :
Sets a new attribute or changes the value of an existing attribute on an element.
```js
document.getElementById("image").setAttribute("src", "someImageLink.png");
```

##### Other Properties/Methods

- `window.getComputedStyle(‘x’)` : gets the final style instead of the inline styles in html. It is a window method(not document)

- `parentElement` : gets the parent of the selected object

- `children` : returns an HTML collection of all children of the selected object.

- `nextElementSibling `: returns the next adjacent sibling.

- `previousElementSibling` : returns the previous adjacent sibling.

