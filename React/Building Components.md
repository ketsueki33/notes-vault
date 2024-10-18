In React, components are reusable, independent pieces of code that define specific parts of a user interface (UI). They are similar to JavaScript functions, but they work in isolation and return HTML elements.

#### Creating a Component

> [!NOTE] 
> It is good practice to place all your components inside a separate folder in `src`usually called `components`.

The component will be in a new `.tsx` file that has the same name as that of the function component.

React requires that the first letter of components be capitalized. JSX will use this capitalization **to tell the difference between an HTML tag and a component instance**. If the first letter of a name is capitalized, then JSX knows it's a component instance; if not, then it's an HTML element.

**Eg) A component that renders a list:**
```jsx title="ListGroup.tsx"
function ListGroup() {
    return (
        <ul className="list-group">
            <li className="list-group-item">An item</li>
            <li className="list-group-item">A second item</li>
            <li className="list-group-item">A third item</li>
            <li className="list-group-item">A fourth item</li>
            <li className="list-group-item">And a fifth one</li>
        </ul>
    );
}

export default ListGroup;
```


> [!NOTE] Why `className` instead of `class`?
> JSX is an extension of JavaScript, and in JavaScript, the term class is a reserved keyword used for defining classes in object-oriented programming. To avoid a conflict with the JavaScript keyword, JSX uses className for defining the CSS class of an element.

#### Fragments
In React, a component can not return more than one element.
For example:
```jsx
return (
    <h1>Helloooo World!</h1>
    <h1>Byee World!</h1>
);
```
Here, we are trying to return two elements, which will give an error. One way to overcome this is to wrap the entire markup in another element.
```jsx
return (
       <div>
           <h1>Helloooo World!</h1>
           <h1>Byee World!</h1>
       </div>
   );
```

But then we are adding one extra element just to make React happy. A better way is to use  **Fragment**. To use fragment, you have to import it from React:-
```jsx
import { Fragment } from "react";
```
and then we use ‘Fragment’ instead of ‘div’
```jsx
return (
       <Fragment>
           <h1>Helloooo World!</h1>
           <h1>Byee World!</h1>
       </Fragment>
   );
```

When this component is rendered on screen, we not going to have extra elements on the DOM.

There is a better way to use fragment without importing it from react. Just use empty angle brackets instead of `<Fragment> </Fragment>`.
```jsx
return (
    <>
        <h1>Helloooo World!</h1>
        <h1>Byee World!</h1>
    </>
);
```
These empty angle brackets tell React to use a Fragment to wrap all the children elements.

#### Rendering Lists
The `ListGroup` component we created in the example earlier is not useful as the items are hard coded. To dynamically display elements of an array in a list, we have to use the [[../JavaScript/Arrays#map( )|map( )]] method as we can’t use `for` loops in JSX.

**Ex) rendering items from an `items` array:**
```jsx
function ListGroup() {
   const items = ["Mumbai", "Bangalore", "Chennai", "Kochi"];
   return (
       <>
           <h1>MyList</h1>
           <ul className="list-group">
               {items.map((item) =>
                   <li key={item} className="list-group-item">{item}</li>
               )}
           </ul>
       </>
   );
}
```

Notice how we added a **key** property to each list item. It was so that React can keep track of the items so that later when we add or remove items dynamically, React knows what part of the page should be updated.

The key can be anything unique. Usually in API data, there will be an `id` property. here we simply use the item itself as there is no `id`.

#### Conditional Rendering
We cannot use `if` statements inside JSX markup. We can use `if` statements outside JSX and return two separate markups based on conditions. But this can result in duplication of code. 

It is better to use logical operations ( or ternary operators ) as shown in below example:
```jsx
return (
    <>
        <h1>MyList</h1>
        {items.length === 0 ? (
            <p>List is empty</p>
        ) : (
            <ul className="list-group">
                {items.map((item) => (
                    <li key={item} className="list-group-item">
                        {item}
                    </li>
                ))}
            </ul>
        )}
    </>
);
```

#### Event Handling
React provides a number of built-in event handling functions that you can use in your components to respond to various user interactions.

These event handling functions are called synthetic events because they abstract away differences in the native browser event handling between different browsers. Here are some of the common event functions available in React:

1. *onClick*: This event is triggered when an element is clicked. 
2. *onDoubleClick*: Triggered when an element is double-clicked.    
3. *onMouseEnter*: Fired when the mouse pointer enters an element.    
4. *onMouseLeave*: Fired when the mouse pointer leaves an element.    
5. *onMouseDown*: Triggered when a mouse button is pressed over an element.    
6. *onMouseUp*: Fired when a mouse button is released over an element.    
7. *onMouseMove*: Fired when the mouse pointer moves within an element.    
8. *onKeyDown*: Triggered when a key is pressed down.    
9. *onKeyUp*: Fired when a key is released.    
10. *onKeyPress*: Triggered when a key is pressed.    
11. *onChange*: Typically used with form elements (like input fields and select boxes) and is triggered when the element's value changes.    
12. *onSubmit*: Triggered when a form is submitted.    
13. *onFocus*: Fired when an element receives focus.    
14. *onBlur*: Triggered when an element loses focus.    
15. *onLoad*: Fired when an image or other media element finishes loading.
16. *onError*: Triggered when an error occurs in loading an image or media element.**

**We will understand event handling using the ‘onClick’ event:**

```tsx
<button onClick={() => alert("Button clicked!")}>Click Me</button>;
```
This will give an alert every time the button is clicked.

This arrow function can have a parameter that represents a browser event:
```tsx
<button onClick={(event) => console.log(event)}>Click Me</button>;
```

We can move this logic to a separate function if it gets complex ( the function remains inside the component function).
```jsx
const handleClick = (event) => {
   console.log(event);
};
```

This would have been valid in JavaScript, but since we are using TypeScript, we need to specify the type of `event`. 

In the inline implementation, the TS compiler knows the type of our parameter, so we don’t get any warnings. In this case, we are declaring a new function and the TS compiler does not know where we will use this so it does not know the type of the parameter. 

To specify the type:
```tsx
import { MouseEvent } from "react";
```
```tsx
const handleClick = (event : MouseEvent) => {
   console.log(event);
};
```

**This function is called an event handler.**

In our `ListGroup.tsx` it would look something like:
```tsx title="ListGroup.tsx"
import { MouseEvent } from "react";

function ListGroup() {
    const items = ["Mumbai", "Bangalore", "Chennai", "Kochi"];

    // event handler
    const handleClick = (event: MouseEvent) => {
        console.log(event);
        alert("Clicked");
    };

    return (
        <>
            <h1>MyList</h1>
            {items.length === 0 && <p>List is empty</p>}
            <ul className="list-group">
                {items.map((item) => (
                    <li onClick={handleClick} key={item} className="list-group-item">
                        {item}
                    </li>
                ))}
            </ul>
        </>
    );
}

export default ListGroup;
```


> [!NOTE] 
> **We are passing the reference to our function instead of calling it. This is because you want to assign a reference to the function to the event handler, not the result of the function call.**

#### Component State
We can declare variables to keep track of things like the last clicked item from the list in our example. 

Suppose we want to highlight the last clicked item, we can try doing so by setting the item’s class as active (from bootstrap)  and updating the last clicked item when an item is clicked.

```tsx
let selectedIndex = 0;
```
```tsx
<li
   onClick={() => {
       selectedIndex = index;
   }}
   key={item}
   className={
       selectedIndex === index ? "list-group-item active" : "list-group-item"
   }
>
   {item}
</li>;
```

But this code won’t update the highlighting of the last clicked item and it will stay on the 0th index. It is because the `selectedIndex` is a variable local to this function and **React doesn’t track this variable**.

We have to use  the [[Hooks#useState|useState]] hook to tell React that a variable will have state/data that will change overtime.

**Implementing `useState` in our example:**
```tsx title="ListGroup.tsx" hl:1,6,16
import { useState } from "react";


function ListGroup() {
   let items = ["Mumbai", "Bangalore", "Chennai", "Kochi"];
   const [selectedIndex, setSelectedIndex] = useState(-1);

   return (
       <>
           <h1>MyList</h1>
           {items.length === 0 && <p>List is empty</p>}
           <ul className="list-group">
               {items.map((item, index) => (
                   <li
                       onClick={() => {
                           setSelectedIndex(index);
                       }}
                       key={item}
                       className={
                           selectedIndex === index
                               ? "list-group-item active"
                               : "list-group-item"
                       }
                   >
                       {item}
                   </li>
               ))}
           </ul>
       </>
   );
}

```

#### Props
Properties can be used to pass data to components to make them reusable. **Props are the inputs to our components**. 

##### Passing Data via Props

First we have to decide the structure of our input object. To do this we use one of the TypeScript features called `interface`. 

To accept Props in our `ListGroup` example:
```tsx title="ListGroup.tsx" hl:3-5,8
import { useState } from "react";

interface Props {
   items: string[];
   heading: string;
}

function ListGroup( {items, heading} : Props ) {
 
   const [selectedIndex, setSelectedIndex] = useState(-1);

   return (
       <>
           <h1>{heading} </h1>
           {items.length === 0 && <p>List is empty</p>}
           <ul className="list-group">
               {items.map((item, index) => (
                   <li
                       onClick={() => {
                           setSelectedIndex(index);
                       }}
                       key={item}
                       className={
                           selectedIndex === index
                               ? "list-group-item active"
                               : "list-group-item"
                       }
                   >
                       {item}
                   </li>
               ))}
           </ul>
       </>
   );
}
```

Now to pass this data from the parent component:
```tsx
return (
   <div>
	   <ListGroup items={items} heading="Cities"/>
	</div>
);
```

##### Passing Functions via Props
Functions can also be passed via Props.

This allows the **parent component to react** after a child component is updated/changed. In our example, we can use this to track which item is selected.

The function to be passed is defined in the parent component and then passed as a prop:
```tsx
function App() {
    const items = ["Mumbai", "Bangalore", "Chennai", "Kochi"];

    const handleSelectItem = (item: string) => {
        console.log(item);
    };
    return (
        <div>
            <ListGroup items={items} heading="Cities" onSelecting={handleSelectItem} />
        </div>
    );
}
```

We will first add the type of function to our interface and then we can call this function inside the child component ( for our example, in `ListGroup.tsx`):
```tsx title="ListGroup.tsx" hl:6,9,21
import { useState } from "react";

interface Props {
    items: string[];
    heading: string;
    onSelecting: (item: string) => void;
}

function ListGroup({ items, heading, onSelecting }: Props) {
    const [selectedIndex, setSelectedIndex] = useState(-1);

    return (
        <>
            <h1>{heading} </h1>
            {items.length === 0 && <p>List is empty</p>}
            <ul className="list-group">
                {items.map((item, index) => (
                    <li
                        onClick={() => {
                            setSelectedIndex(index);
                            onSelecting(item);
                        }}
                        key={item}
                        className={
                            selectedIndex === index
                                ? "list-group-item active"
                                : "list-group-item"
                        }
                    >
                        {item}
                    </li>
                ))}
            </ul>
        </>
    );
}

export default ListGroup;
```
Here, the `onSelecting` function will be called every time we click on an item in the list i.e every time the `onClick` event is triggered.

##### Optional Props and default Values
We can use `?` to tell TS compiler that a prop is optional:
```tsx
interface Props {
   children: string;
   onClick: () => void;
   color?: string;
}
```

We can set  default values while destructuring. This will set the value of a property to the default value if that property is missing. 

eg) Here, the default value of ‘color’ is  set to “primary”
```tsx
function Button({ children, onClick, color = "primary" }: Props) {
   return (
       <button
           typeof="button"
           className={"btn btn-" + color}
           onClick={onClick}
       >
           {children}
       </button>
   );
}
```

**Restricting the acceptable values for Props**
We can restrict the values that are allowed in our interface so that we will get an error if we pass on the wrong inputs.

in the above button example, we can can set the acceptable values for ‘color’ like this:
```tsx
interface Props {
   children: string;
   onClick: () => void;
   color?: "primary" | "danger" | "warning";
}
```

##### Props vs State

| *Props*                                                                            | *State*                                                                                              |
| ---------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------- |
| It is the input passed to a component. Similar to function arguments.              | It is the internal data managed by a component that can change overtime. Similar to local variables. |
| Props are treated as immutable. We do not change their value inside the component. | It is mutable. In fact, that is the purpose of it i.e to keep track of changes.                      |
| Anytime its value changes, React will not re-render the component.                 | Anytime its value changes, React will re-render the component and update the DOM.                    |
##### Passing Children as Props
We can pass text as a child to this component. To be able to pass text like shown below:
```tsx
return (
       <div className="alert alert-primary">
           <Alert>
             Hello
           </Alert>
       </div>
   );
```

We have to make use of **a special Prop that all components support called `children`**. Now, we can pass text as a child to our component:
```tsx
interface Props {
   children: string;
}


function Alert({ children }: Props) {
   return <div> {children} </div>;
}
```

We can also pass HTML content as children to the component. To do this, we have to **set the type of children in the Props interface as `ReactNode`**. It should be imported first as shown below:
```tsx
import { ReactNode } from "react";


interface Props {
   children: ReactNode;
}


function Alert({ children }: Props) {
   return <div> {children} </div>;
}
```

This will allow us to pass HTML content as children as shown in the example below:
```tsx
return (
       <div className="alert alert-primary">
           <Alert>
             <small>Hello</small> <strong>World</strong>
           </Alert>
       </div>
   );
```