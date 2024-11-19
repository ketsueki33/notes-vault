In React, we would have have a separate client-side which would make a call to the backend-server which would then query the database server. But in Next.js we don't have to do this as we can make use of Server Actions. 

> [!Info] Server Actions is not Compulsory
> You are free to choose between server actions, client-side fetching, or API routes depending on the use case.

**What are Server Actions?**
Server Actions are functions that run on the server, but that can be called from the client, just like a normal function. This allows us to run code on the server without having to create an API endpoint.

**What can you do with Server Actions?**
- *writing to a database*: you can write to a database directly from the client, without having to create an API endpoint.
- *server logic*: executing any server-related business logic, such as sending emails, creating files etc.
- *calling external APIs*: you can call external APIs directly from server actions ,without having to create an API endpoint.

**Advantages of Server Actions:**
1. *No need to create an API endpoint*: you can run server code without having to create an API endpoint.
2. *Jumping to the definition*: you can jump to the definition of a server action just by clicking on it in your code editor, without the need of searching for it in your codebase.
3. *Type safety*: you can use TypeScript to define the arguments and return value of your server actions, and Next.js will automatically validate them for you.
4. *Less code*: you can write less code, as you need a lot less boilerplate to run server code - you can just define a function and its parameters - and then call it from the client.

#### Defining a Server Action
##### Single Server Action in a server component
If you are defining a server action in a server component, the only thing you need to do is to define a function with the `use server` keyword at the top.
```tsx
async function myActionFunction() {
  'use server';

  // do something
}
```

##### Multiple Server Actions in a separate file
An alternative way to define a server action is to use export multiple functions from a file, adding the `use server` keyword at the top of the file. **If you want to use Server Actions in a client component, then you will have to use this method and export the function**
```ts
'use server'
 
export async function myActionFunction() {
  // do something
}
 
export async function anotherActionFunction() {
  // do something
}
```

> [!important] Client Components and Server Actions
> **Client Components can only import actions** from server actions files: client components cannot define server actions inline from the same file - but you can still import them from a file that defines multiple server actions using the `use server` keyword.

#### Invoking Server Actions
Simply import the Server Action ( in case of client component) and use it like any other function.
```tsx
'use client'
 
import { create } from '@/app/actions' // where `create` is a server action
 
export function Button() {
  return <Button onClick={create} />
}
```


##### Invoking a Server Action from a Form
React extends the HTML `<form>` element to allow Server Actions to be invoked with the `action` prop.

When invoked in a form, the action automatically receives the `FormData` object. You don't need to use React `useState` to manage fields, instead, you can extract the data using the native [[https://developer.mozilla.org/en-US/docs/Web/API/FormData#instance_methods]]:
```tsx
export default function Page() {
  async function createInvoice(formData: FormData) {
    'use server'
 
    const rawFormData = {
      customerId: formData.get('customerId'),
      amount: formData.get('amount'),
      status: formData.get('status'),
    }
 
    // mutate data
    // revalidate cache
  }
 
  return <form action={createInvoice}>...</form>
}
```

