We can create API endpoints in our NextJS server by using **route handlers**.  

Route handlers are defined in a `route.ts` file inside the app directory.
Eg) Route handler for `/api` route will be defined in `/app/api/route.ts`.

**Things to remember:**
- Route Handlers can be nested inside the `app` directory, similar to `page.tsx` and `layout.tsx`. 
- There **cannot** be a `route.ts` file at the same route segment level as `page.tsx`.

##### Supported HTTP methods:
- `GET`
- `POST`
- `PUT`
- `PATCH`
- `DELETE`
- `HEAD`
- `OPTIONS`

##### Functions for methods
You have to create a function with same name as the HTTP method you want to implement for that route. Arrow functions also work.

eg) `GET` method for `products/api`
```ts title="app/products/api/route.ts"
export async function GET(request: Request) {
  const { searchParams } = new URL(request.url)
  const id = searchParams.get('id')
  const res = await fetch(`https://data.mongodb-api.com/product/${id}`, {
    headers: {
      'Content-Type': 'application/json',
      'API-Key': process.env.DATA_API_KEY!,
    },
  })
  const product = await res.json()
 
  return Response.json({ product })
}
```

eg) `POST` method for `/api/chatgpt`
```ts title="app/api/chatgpt/route.ts"
import { NextResponse } from "next/server";
import { GoogleGenerativeAI } from "@google/generative-ai";

export const POST = async (request: Request) => {
    const { tag } = await request.json();

    try {
        const genAI = new GoogleGenerativeAI(process.env.GEMINI_API_KEY!);
        const model = genAI.getGenerativeModel({ model: "gemini-1.5-flash" });

        const prompt = `Give a brief description of ${tag} in about 30 words in the context of programming.`;

        const result = await model.generateContent(prompt);
        console.log("RESULT", result.response.text());

        return NextResponse.json({ reply: result.response.text() });
    } catch (error) {
        return NextResponse.json({ error });
    }
};
```
