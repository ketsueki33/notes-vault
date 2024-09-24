There are a lot of things to take care of when implementing Authentication:
- Authentication methods:
	- Username/Password
	- Social logins such as Google, GitHub etc.
	- Multi-factor Authentication
- Session Management
	- Session tokens
	- Session Validation
	- Session Invalidation
- Authorization
	- Access Control
	- Authorization Checks for each request
- User Management
	- Password Reset
	- Account Lockout



There are now many different libraries that simplify Authentication. 
Eg) Passport, OAuth, NextAuth, Clerk etc.

We will explore **Clerk**.

It claims to have very good integration with Next.js App Router.

[Clerk Documentation for NextJS](https://clerk.com/docs/quickstarts/nextjs)

#### Clerk Provider
All Clerk hooks and components must be children of the [`<ClerkProvider>`](https://clerk.com/docs/components/clerk-provider) component, which provides active session and user context. 

Which is why we will wrap our app inside the `<ClerkProvider` component in the root `layout.tsx`:
```tsx
import { ClerkProvider, SignInButton, SignedIn, SignedOut, UserButton } from "@clerk/nextjs";
import { ReactNode } from "react";
import "./globals.css";

export default function RootLayout({ children }: { children: ReactNode }) {
    return (
        <ClerkProvider>
            <html lang="en">
                <body>
                    <header>
                        <SignedOut>
                            <SignInButton />
                        </SignedOut>
                        <SignedIn>
                            <UserButton />
                        </SignedIn>
                    </header>
                    <main>{children}</main>
                </body>
            </html>
        </ClerkProvider>
    );
}
```


> [!info] [Clerk Documentation](https://clerk.com/docs/quickstarts/nextjs) has detailed and easy to follow instructions to get everything running. 

#### Modifying Appearance of Clerk Provider
We can modify the appearance of login and sign-up pages by using the appearance prop of `ClerkProvider` . We have to specify which classes are to be applied to which elements.

```tsx
<ClerkProvider
            appearance={{
                elements: {
                    formButtonPrimary: "primary-gradient",
                    footerActionLink: "primary-text-gradient hover:text-primary-500",
                },
            }}
        >
```

#### Showing Content only to Signed-in Users
We can use the `<SignedIn>` component of clerk to wrap any protected content. The components inside this wrapper will only be shown to signed-in users. For example, showing the User Button ( another component of clerk ) only to Signed-in users:
```tsx
<SignedIn>
    <UserButton
        afterSignOutUrl="/"
        appearance={{
            elements: {
                avatarBox: "h-10 w-10",
            },
            variables: { colorPrimary: "#FF7000" },
        }}
    />
</SignedIn>
```

Similarly, `<SignedOut>` component can be used to show content only to user's who haven't signed in.

### Clerk Webhooks
Clerk webhooks allow you to receive event notifications from Clerk, such as when a user is created or updated. When an event occurs, Clerk will send an HTTP `POST` request to your webhook endpoint configured for the event type. The payload carries a JSON object. You can then use the information from the request's JSON payload to trigger actions in your app, such as sending a notification or updating a database.

#### Syncing data with backend using webhooks
[refer this](https://clerk.com/docs/integrations/webhooks/sync-data)

**Example of api for clerk webhook**
```ts title=app/api/webhook/route.ts
/* eslint-disable camelcase */
import { Webhook } from "svix";
import { headers } from "next/headers";
import { WebhookEvent } from "@clerk/nextjs/server";
import { createUser, deleteUser, updateUser } from "@/lib/actions/user.action";
import { NextResponse } from "next/server";

export async function POST(req: Request) {
    // You can find this in the Clerk Dashboard -> Webhooks -> choose the endpoint
    const WEBHOOK_SECRET = process.env.NEXT_CLERK_WEBHOOK_SECRET;

    if (!WEBHOOK_SECRET) {
        throw new Error(
            "Please add WEBHOOK_SECRET from Clerk Dashboard to .env or .env.local",
        );
    }

    // Get the headers
    const headerPayload = headers();
    const svix_id = headerPayload.get("svix-id");
    const svix_timestamp = headerPayload.get("svix-timestamp");
    const svix_signature = headerPayload.get("svix-signature");

    // If there are no headers, error out
    if (!svix_id || !svix_timestamp || !svix_signature) {
        return new Response("Error occured -- no svix headers", {
            status: 400,
        });
    }

    // Get the body
    const payload = await req.json();
    const body = JSON.stringify(payload);

    // Create a new Svix instance with your secret.
    const wh = new Webhook(WEBHOOK_SECRET);

    let evt: WebhookEvent;

    // Verify the payload with the headers
    try {
        evt = wh.verify(body, {
            "svix-id": svix_id,
            "svix-timestamp": svix_timestamp,
            "svix-signature": svix_signature,
        }) as WebhookEvent;
    } catch (err) {
        console.error("Error verifying webhook:", err);
        return new Response("Error occured", {
            status: 400,
        });
    }

    // Do something with the payload
    // For this guide, you simply log the payload to the console

    const eventType = evt.type;

    if (eventType === "user.created") {
        const {
            id,
            email_addresses,
            image_url,
            username,
            first_name,
            last_name,
        } = evt.data;

        const mongoUser = await createUser({
            clerkId: id,
            name: `${first_name}${last_name ? ` ${last_name}` : " "}`,
            username: username!,
            email: email_addresses[0].email_address,
            picture: image_url,
        });

        return NextResponse.json({ message: "OK", user: mongoUser });
    }

    if (eventType === "user.updated") {
        const {
            id,
            email_addresses,
            image_url,
            username,
            first_name,
            last_name,
        } = evt.data;

        const mongoUser = await updateUser({
            clerkId: id,
            updateData: {
                name: `${first_name}${last_name ? ` ${last_name}` : " "}`,
                username: username!,
                email: email_addresses[0].email_address,
                picture: image_url,
            },
            path: `profile/${id}`,
        });

        return NextResponse.json({ message: "OK", user: mongoUser });
    }

    if (eventType === "user.deleted") {
        const { id } = evt.data;

        const deletedUser = await deleteUser({
            clerkId: id!,
        });

        return NextResponse.json({ message: "OK", user: deletedUser });
    }

    return new Response("", { status: 200 });
}
```

