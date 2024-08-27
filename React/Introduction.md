
#### What is React?
React is a JavaScript library for building dynamic and interactive UI.

JavaScript( Vanilla ) is used to manipulate items in the DOM ( Document object Model) and change the page content in response to user actions. But as our application grows, working with the DOM can become quite complex and challenging.

This is where React comes in. With React, we don’t have to worry about querying and updating DOM elements. Instead, we describe a web page using small reusable components and React will take care of creating and updating DOM elements.

**Components** help us write reusable, modular and better organised code.
#### Creating a React app
We will be using [Vite](https://vitejs.dev/guide/) to create React apps since it is faster than CRA( the official tool ).

To create a new app using Vite, go to the folder you want to start your project in and run the following command:
```bash
npm create vite@latest
```

You will need to specify the project name, select react as the framework, and then select a language ( we have used TypeScript). Now run:
```bash
cd <project-name>
npm install
npm run dev
```

#### Creating a React Component
To create a react component, add a file with the component name in the source folder `src`. We are using TypeScript so the extension of this file would be `.ts`  or `.tsx`. Generally, `.ts` is used for plain TypeScript files and `.tsx` for React components.

There are two types of  React components :-
1. Class Based 
2. Function based

Function based components are more concise and easier to write. 

> [!NOTE]
>  In React you must name your component in PascalCase. PascalCase is a naming convention in which the first letter of each word in a compound word is capitalized. Software developers often use PascalCase when writing source code to name functions, classes, and other objects. PascalCase is similar to camelCase, except the first letter in PascalCase is always capitalized.

eg) `Message.tsx`
```tsx
function Message(){
   return <h1>Hello World!</h1>
}

export default Message;
```

This syntax with HTML embedded in JavaScript is called JSX (JavaScript XML). This code will be compiled into JavaScript.

##### `<App>` Component
The main component which contains all other components is the `App` component which is defined in `App.tsx`.

To use the `<Message>` component that we defined earlier ( in `App.tsx`):
```tsx
import Message from "./Message";


function App() {
 const name = "ketsueki";
 if( name )
   return <h1> Hello, {name} </h1>;
 else
   return <div><Message/></div>;
}


export default App;
```

#### How React Works
Our component tree in the previous example consists of a top level component `App` and a child component `Message`.

When our application starts, React takes this component tree and builds a JavaScript data structure called ‘Virtual DOM’. It is different from the actual DOM in the browser. It is a lightweight in-memory representation of our component tree where each node represents a component and its properties. When the state or the data of a component changes, React updates the corresponding node in the Virtual DOM to reflect the new state.

Then it compares the current version of virtual DOM with the previous version to identify the nodes that should be updated, it will then update those nodes in the actual DOM.

This updating is not done by React itself. Instead it’s done by a companion library - `ReactDOM`

#### React Ecosystem
React is a tool ( library ) that is only used for making UI. We need additional tools to build modern applications such as for Routing, making HTTP requests, Managing app state, Form Validation etc.

Good thing about React is that it does not have any restrictions on which tools we can use for these additional concerns.
