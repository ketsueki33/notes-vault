
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

##### `useSearchParams()`
`useSearchParams` is a **Client Component** hook that lets you read the current URL's **query string**.
```tsx
'use client'
 
import { useSearchParams } from 'next/navigation'
 
export default function SearchBar() {
  const searchParams = useSearchParams()
 
  const search = searchParams.get('search')
 
  // URL -> `/dashboard?search=my-project`
  // `search` -> 'my-project'
  return <>Search: {search}</>
}
```
#### Components provided by NextJS

##### `<Link>`
`<Link>` is a React component that extends the HTML `<a>` element to provide [prefetching](https://nextjs.org/docs/app/building-your-application/routing/linking-and-navigating#2-prefetching) and client-side navigation between routes. It is the primary way to navigate between routes in Next.js.

```tsx
import Link from 'next/link'
 
export default function Page() {
  return <Link href="/dashboard">Dashboard</Link>
}
```

##### `<Image>`
The Next.js Image component extends the HTML `<img>` element with features for automatic image optimization:

- **Size Optimization:** Automatically serve correctly sized images for each device, using modern image formats like WebP and AVIF.
- **Visual Stability:** Prevent [layout shift](https://nextjs.org/learn/seo/web-performance/cls) automatically when images are loading.
- **Faster Page Loads:** Images are only loaded when they enter the viewport using native browser lazy loading, with optional blur-up placeholders.
- **Asset Flexibility:** On-demand image resizing, even for images stored on remote servers

```tsx
import Image from 'next/image'
 
export default function Page() {
  return (
    <Image
      src="https://s3.amazonaws.com/my-bucket/profile.png"
      alt="Picture of the author"
      width={500}
      height={500}
    />
  )
}
```
