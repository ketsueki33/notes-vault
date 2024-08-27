### Client vs Server Paradigm

![[_Architecture of NextJS]]

**Benefits of server-side rendering**
- Smaller JavaScript bundle size.
- Enhanced SEO.
-  Faster initial page load for better accessibility and user experience.
- Efficient utilization of server resources.  

**Where to use client components?**
If a component needs the user to interact with it, such as:
- Clicking buttons
- Entering information in input fields
- Triggering events
- Using React hooks
Then it should be a client component.

**When to use server components?**
If a component does not require any user interaction, such as:
- Fetching data from a server
- Displaying static content
- Performing server-side computations.
Then it should be a server component.

| Purpose                                                                       | Server Component | Client Component |
| ----------------------------------------------------------------------------- | ---------------- | ---------------- |
| Fetch Data                                                                    | ✅                | ❌                |
| Access backend resources directly                                             | ✅                | ❌                |
| Keep sensitive information on the server( access token , API Keys etc.)       | ✅                | ❌                |
| Keep large dependencies on the server / Reduce client-side Javascript         | ✅                | ❌                |
| Add interactivity and event listeners ( onClick(), onChange() etc.)           | ❌                | ✅                |
| Use State and Life cycle effects( useState(), useReducer(), useEffect() etc.) | ❌                | ✅                |
| Use browser-only APIs                                                         | ❌                | ✅                |
| Use custom hooks that depend on state, effects or browser-only APIs           | ❌                | ✅                |
| Use React Class components                                                    | ❌                | ✅                |

> [!important]
> All components are automatically considered Server components unless specified otherwise. 

**How to specify a client component?**
We have to write this at the top of the component file:
```tsx
"use client"
```
When we use `"use client"` in a file, all the other modules imported into that file, including child server components, are treated as part of the client module.

The docs says any child of a client component is considered part of the client bundle.
If it is a child by being imported into the parent, then yes. If it is a child by being passed in as a children prop, then no. 

> [!NOTE]
> Only a child component being imported directly into the client component will be considered a client component. If the child component is being passed as  children to the client component, then the child component will still be a server component.
> 
> *Child by import:*
> ```jsx
> 'use client'
> 
> import { useState } from 'react'
> import ChildComponent from './ChildComponent'
> 
> export default function ParentComponent() {
>   const [count, setCount] = useState(0)
> 
>   return (
>     <div>
>       <h1>Parent Component</h1>
>       <button onClick={() => setCount(count + 1)}>Increment</button>
>       <ChildComponent count={count} />
>     </div>
>   )
> }
> ```
> In this case, `ChildComponent` is imported directly into the client component `ParentComponent`. This means `ChildComponent` will be part of the client bundle and will be rendered on the client side, regardless of whether it has a 'use client' directive or not.
> 
> *Child passed as prop:*
> ```jsx
> 'use client'
> 
> import { useState } from 'react'
> 
> export default function ParentComponent({ children }) {
>   const [count, setCount] = useState(0)
> 
>   return (
>     <div>
>       <h1>Parent Component</h1>
>       <button onClick={() => setCount(count + 1)}>Increment</button>
>       {children}
>     </div>
>   )
> }
> ```
> ```jsx
> // In a page or layout file
> import ParentComponent from './ParentComponent'
> import ServerComponent from './ServerComponent'
> 
> export default function Page() {
>   return (
>     <ParentComponent>
>       <ServerComponent />
>     </ParentComponent>
>   )
> }
> ```
> In this case, `ServerComponent` is passed as a child to `ParentComponent`. Even though `ParentComponent` is a client component, `ServerComponent` can still be a server component. It will be rendered on the server and then passed to the client component as static HTML.

**Pre-rendering in NextJS**
NextJS performs pre-rendering of certain content before sending it back to client. This means-
- Server components are guaranteed to be only rendered on the server.
- Client components are primarily rendered on the client side. However, NextJS also pre-renders them on the server for better UX and SEO.


### Different Rendering Strategies
Once the compilation process is complete, which involves converting code from a higher-level programming language to a lower-lever representation (binary code), our application goes through two crucial phases: Build Time and Run Time.

- **Build Time:** The period when **source code is translated** into a machine-readable format (like an executable). It's like preparing the ingredients and instructions for a recipe.
- **Runtime:** The period when the **translated code is executed** by the computer to perform its tasks. It's like actually baking the cake from the recipe.

**NextJS provides us with two different runtimes to execute our code**
1. **Node.js runtime**
	Default runtime that has access to all Node.js APIs and the ecosystem.
2. **The Edge runtime**
	A lightweight runtime based on Web APIs with support to a limited subset of Node.js APIs. 

NextJS offers the flexibility of choosing the runtime. You can do so by putting this at the top of your file:
```tsx
export const runtime = 'edge' // 'nodejs'(default)
```

Depending on the above-discussed factors, such as the rendering environment, and the time period ( build and run time ), ==NextJS provides three strategies for rendering on the server:==

#### 1. Static Site Generation
SSG happens at build time on the server.  

During the build process, the content is generated and converted into HTML, CSS and JS files. It doesn't require server interaction during runtime. The generated files can be hosted on a CDN and the served to the client as-is.

The result, the rendered content, is cached and reused on subsequent requests leading to fast content delivery and less server load. This minimal processing results in higher performance. 

Although SSG handles dynamic data during the build process, it requires a rebuild if you update anything, as it happens during the build time.
#### 2. Incremental Static Regeneration
It allows us to update these static pages after we build them without needing to rebuild the entire site. 

The on-demand generation of ISR allows us to generate a specific page on-demand or in response to a user's request. Meaning, a certain part of the websites or pages will be rendered at build time while other is generated only when needed, i.e , at run time.  

This reduces the build time and improves the overall performance of the website by updating only requested pages of regeneration. 

With this hybrid strategy, we now have the flexibility to manage content updates. We can cache the static content as well as re-validate them if needed.

#### 3. Server Side Rendering
Dynamic Rendering that enables the generation of dynamic content for each request, providing fresh and interactive experiences. 

SSR involves heavy server-side processing, where the server executes code for every individual request, generates the necessary HTML, and delivers the response along with the required JavaScript code for client-side interactivity.
##### Why do we need SSR ( over SSG and ISR )?
SSR excels in situations where a website heavily relies on client-side interactivity and requires real-time updates. It is particularly well-suited for authentication, real-time collaborative applications such as chat platforms, editing tools and video streaming services.

The benefits of real-time interactivity and up-to-date content make SSR a valuable choice for specific application requirements. 

#### Choosing the rendering strategy
We have the freedom to choose any of these rendering techniques for any part of our application code as per our specific requirements.

==NextJS uses SSG rendering by default.== The flexibility of NextJS allows us to pick the most suitable rendering approach for each page of our website.

![[_Rendering Strategies|700]]
