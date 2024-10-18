#### Loading.tsx
The special file `loading.tsx` helps you create meaningful Loading UI with [React Suspense](https://react.dev/reference/react/Suspense). With this convention, you can show an [instant loading state](https://nextjs.org/docs/app/building-your-application/routing/loading-ui-and-streaming#instant-loading-states) from the server while the content of a route segment loads. The new content is automatically swapped in once rendering is complete.


> [!info] React Suspense
> `<Suspense>` lets you display a fallback until its children have finished loading.


##### using `<Suspense/>` in `layout.tsx`

```tsx title="app/(root)/loading.tsx"
const Loading = () => {
    // You can add any UI inside Loading, including a Skeleton.
    return <LoadingSkeleton />;
};

export default Loading;
```
```tsx title="app/(root)/layout.tsx" hl=9
const layout = ({ children }: { children: ReactNode }) => {
    return (
        <main className="background-light850_dark100 relative">
            <Navbar />
            <div className="flex">
                <LeftSidebar />
                <section className="flex min-h-screen flex-1 flex-col px-6 pb-6 pt-36 max-md:pb-14 sm:px-14">
                    <div className="mx-auto w-full max-w-5xl">
                        <Suspense fallback={<Loading />}>{children}</Suspense>
                    </div>
                </section>
                <RightSidebar />
            </div>
            Toaster
        </main>
    );
};
```

##### without `<Suspense/>`
You can have a loading state for a page without using `Suspense` or making any changes in `layout.tsx`. 

Simply, create a `loading.tsx` file beside the `page.tsx` and NextJS will use the component exported by `loading.tsx` as a default fallback while the page loads.   This way you don't have to create a `layout.tsx` for every route in you app directory.


> [!NOTE] `loading.tsx` without `page.tsx`
> You can  create a `loading.tsx` file in the parent folder, even if there is no `page.tsx` at that specific level (`page.tsx` is in sub routes). Next.js will use the `loading.tsx` file from the closest parent folder when rendering a route.


It will still be rendered as a child of the closest `layout.tsx` file.

**Example)**
```tsx title="app/(root)/(home)/loading.tsx"
import { Skeleton } from "@/components/ui/skeleton";

const Loading = () => {
    return (
        <>
            <div className="flex w-full justify-between">
                <h1 className="h1-bold text-dark100_light900">All Questions</h1>
                <Skeleton className="w-[150px]" />
            </div>
            <div className="mt-11 flex flex-col gap-3 sm:flex-row md:flex-col">
                <Skeleton className="h-8 w-full" />
                <Skeleton className="h-8 w-full sm:w-[250px] md:hidden" />
                <div className="hidden gap-2 md:flex">
                    {[1, 2, 3, 4].map((e) => (
                        <Skeleton key={e} className="h-8 w-[120px]" />
                    ))}
                </div>
            </div>

            <div className="mt-10 flex w-full flex-col gap-6">
                {[1, 2, 3].map((e) => (
                    <Skeleton className="h-[180px] w-full" key={e} />
                ))}
            </div>
        </>
    );
};

export default Loading;
```

#### Streaming
**Streaming** allows you to break down the page's HTML into smaller chunks and progressively send those chunks from the server to the client.

This enables parts of the page to be displayed sooner, without waiting for all the data to load before any UI can be rendered.

Streaming is particularly beneficial when you want to prevent long data requests from blocking the page from rendering as it can reduce the [Time To First Byte (TTFB)](https://web.dev/ttfb/) and [First Contentful Paint (FCP)](https://web.dev/first-contentful-paint/). It also helps improve [Time to Interactive (TTI)](https://developer.chrome.com/en/docs/lighthouse/performance/interactive/), especially on slower devices.

**Example:**
`<Suspense>` works by wrapping a component that performs an asynchronous action (e.g. fetch data), showing fallback UI (e.g. skeleton, spinner) while it's happening, and then swapping in your component once the action completes.

```tsx title="app/dashboard/page.tsx"
import { Suspense } from 'react'
import { PostFeed, Weather } from './Components'
 
export default function Posts() {
  return (
    <section>
      <Suspense fallback={<p>Loading feed...</p>}>
        <PostFeed />
      </Suspense>
      <Suspense fallback={<p>Loading weather...</p>}>
        <Weather />
      </Suspense>
    </section>
  )
}
```