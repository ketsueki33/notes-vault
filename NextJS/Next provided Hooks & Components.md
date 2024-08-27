
#### Hooks provided by NextJS

##### `useRouter()`
This hook allows you to programmatically change routes inside a Client Component.
```tsx
"use client"
import { useRouter } from 'next/navigation'

export default function Page() {
 const router = useRouter()
 // ...
 router.push('/dashboard') // Navigate to /dashboard
}
```

##### `usePathname()`
A Client Component hook that lets you read the current URL's path name.
```tsx
"use client"
import { usePathname } from 'next/navigation'

export default function Page() {
 const pathname = usePathname() // returns "/dashboard" on /dashboard?foo=bar
 // ...
}
```


#### Components provided by NextJS

##### `<Link>`
`<Link>` is a React component that extends the HTML `<a>` element to provide [[https://nextjs.org/docs/app/building-your-application/routing/linking-and-navigating#2-prefetching]] and client-side navigation between routes. It is the primary way to navigate between routes in Next.js.

```tsx
import Link from 'next/link'
 
export default function Page() {
  return <Link href="/dashboard">Dashboard</Link>
}
```


