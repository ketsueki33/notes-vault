Context is a feature of React that enables components to share data without passing props down manually at every level of the component tree. This is particularly useful for data that can be considered "global" for a tree of React components, such as user authentication status or theme preferences.

Next.js, on the other hand, offers capabilities like server-side rendering and generating static websites, making it a robust solution for modern web development.

However, Context is a hook and hooks are client-side which means attempting to create a context at the root of your NextJS application will result in an error. So how will this work? 

You have to create a context and render its provider inside a separate component. **Wrapping the children prop in the root layout with a context provider like theme that is a client component does not make the entire tree a client boundary( i.e server components down the tree will remain server components)**, since those components are being rendered as children they can be server or client components.

Let's see how context works in Client and Server components:

#### Context in Client Components
In Next.js, context is fully supported within Client Components. You can use all the context APIs, such as `createContext`, `useContext`, and `Provider`, in your Client Components. So it is the same as using Context in React. 

#### Context in Server Components
In Next.js, React Server Components don't support creating or consuming context directly. If you try to create or use a context in a Server Component, it will result in an error. Similarly, rendering a context provider that doesn't have the `"use client"` directive will also cause an error in Server Components.

##### Third-Party Context Providers
You have to create your own Client Component that wraps the third-party provider. Here's an example:

```ts title=app/providers.ts
'use client';

import { ThemeProvider } from 'acme-theme';
import { AuthProvider } from 'acme-auth';

export function Providers({ children }) {
  return (
    <ThemeProvider>
      <AuthProvider>{children}</AuthProvider>
    </ThemeProvider>
  );
}
```

In this example, we create a Client Component called `Providers` that wraps the third-party providers, `ThemeProvider` and `AuthProvider`. We mark this component as a Client Component by using the `"use client"` directive.

Now, we can import and render `<Providers/>` directly within your root layout:
```tsx title=app/layout.tsx
import { Providers } from './providers';

export default function RootLayout({ children }) {
  return (
    <html>
      <body>
        <Providers>{children}</Providers>
      </body>
    </html>
  );
}
```

In the `RootLayout` component, we import and render the `Providers` component, which wraps the entire content of the layout. This allows all the components and hooks from the third-party libraries to work as expected within your own client components.

By rendering the providers at the root level, all the components throughout your app will be able to consume the context provided by the third-party libraries.


> [!info] Note
> You should render providers as deep as possible in the component tree. In the example above, the `Providers` component only wraps `{children}` instead of the entire `<html>` document. This helps Next.js optimize the static parts of your server components.

##### Our Own Context (Theme Context Example)
Let's understand by creating a context that will handle the dark and light mode in our application. 

Since this is a provider, we must add the  **"use client"** directive on top.
```tsx title=ThemeProvider.tsx
"use client";

import { ReactNode, createContext, useContext, useState } from "react";

interface ThemeContextType {
    mode: "dark" | "light";
    toggleTheme: () => void;
}

const ThemeContext = createContext<ThemeContextType>({} as ThemeContextType);

export function ThemeProvider({ children }: { children: ReactNode }) {
    const [mode, setMode] = useState<"dark" | "light">("dark");

    const toggleTheme = () => {
        if (mode === "dark") {
            setMode("light");
            document.documentElement.classList.add("light");
        }
        if (mode === "light") {
            setMode("dark");
            document.documentElement.classList.add("dark");
        }
    };

    return <ThemeContext.Provider value={{ mode, toggleTheme }}>{children}</ThemeContext.Provider>;
}

export function useTheme() {
    const context = useContext(ThemeContext);

    if (context === undefined) {
        throw new Error("useTheme must be used within a Theme Provider");
    }

    return context;
}

```

And now to use it within our layout `app/layout.tsx` :
```tsx
export default function RootLayout({ children }: { children: ReactNode }) {
    return (
        <html lang="en" className={`${inter.variable} ${spaceGrotesk.variable}`}>
            <body>
				<ThemeProvider>{children}</ThemeProvider>
            </body>
        </html>
    );
}
```