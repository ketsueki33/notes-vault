#### Initializing a NextJS project-
```bash
npx create-next-app@latest
```

> [!example] My Config:
> 
> | Setting                        | Option |
> | ------------------------------ | ------ |
> | TypeScript                     | Yes    |
> | ESLint                         | Yes    |
> | TailwindCSS                    | Yes    |
> | 'src/' directory               | No     |
> | App Router                     | Yes    |
> | Customize default import alias | No     |
> 


#### ESLint and Prettier Setup
If we look at `.eslintrc.json` file in the root of our Next.js project, there is already an ESLint config called *next/core-web-vitals* . This warns us when any code in out project will affect core web vitals.

We still need to add another ESLint config for consistent code styling as the default NextJS config doesn't do much to help us improve the quality of our code. 

**standard ESLint config**
installing `eslint-config-standard` will add the standard  ESLint configuration in our project. Then, add `"standard"` to`.eslintrc.json` 

We can test if it is working with `npm run lint`. We will also set it up to run automatically when we save a file.

**ESLint config for TailwindCSS**
`eslint-plugin-tailwindcss` will organize our tailwind classes organized logically and make our code easier to read and review. Add `"plugin:tailwindcss/recommended"` to`.eslintrc.json` 

**Prettier v/s ESLint potential conflicts**
ESLint may sometimes conflict with Prettier. To avoid these conflicts, we should install `eslint-config-prettier`

Some other ESLint plugins will also help.. so overall ESLint setup will be:

###### Overall ESLint setup

**Install**
```bash
npm install -D prettier prettier-plugin-tailwindcss eslint-config-prettier eslint eslint-plugin-tailwindcss @typescript-eslint/parser eslint-plugin-prettier
```

**Update `eslintrc.json`**
```json
{
    "root": true,
    "extends": [
        "next/core-web-vitals",
        "standard",
        "eslint:recommended",
        "plugin:prettier/recommended",
        "plugin:tailwindcss/recommended",
        "prettier"
    ],
    "parser": "@typescript-eslint/parser",
    "overrides": [
        {
            "files": ["*.ts", "*.tsx", "*.js"]
        }
    ],
    "plugins": ["tailwindcss"],
    "rules": {
        "prettier/prettier": "warn"
    }
}
```

**Create `.prettierrc` in the root**
```json
{
    "tabWidth": 4,
    "semi": true,
    "plugins": ["prettier-plugin-tailwindcss"],
    "singleQuote": false
}
```
###### Setting things up in VSCode
Now that everything is setup command-line-wise, we will integrate ESLint and Prettier with VSCode.  For that we will modify the `settings.json` file of the user.
```json
{
	"editor.defaultFormatter": "esbenp.prettier-vscode",
	    "editor.formatOnSave": true,
	    "editor.codeActionsOnSave": {
	        "source.fixAll.eslint": "explicit",
	        "source.addMissingImports": "explicit",
	        "source.organizeImports": "explicit"
	    },
	    "[typescriptreact]": {
	        "editor.defaultFormatter": "esbenp.prettier-vscode"
	    },
}
```

>[!info]+ Alternative: creating a config specific to our project that can be shared with others
>- Create a `.vscode` folder
>- inside that create a `settings.json` file
>- Insert the above code in it. 

#### Tailwind CSS Setup

###### tailwind.config.ts
We will be converting a [style guide](https://www.figma.com/design/2vtjgodtBxTdg0zOUHPvXh/JSM-Pro---DevOverflow?node-id=1-49&t=ha8oMEy6IeAFaaZv-0) into a Tailwind config. 
> [!file]+ tailwind.config.ts
> ```ts
> import type { Config } from "tailwindcss";
> 
> const config: Config = {
>     darkMode: ["class"],
>     content: [
>         "./pages/**/*.{ts,tsx}",
>         "./components/**/*.{ts,tsx}",
>         "./app/**/*.{ts,tsx}",
>         "./src/**/*.{ts,tsx}",
>     ],
>     theme: {
>         container: {
>             center: true,
>             padding: "2rem",
>             screens: {
>                 "2xl": "1400px",
>             },
>         },
>         extend: {
>             colors: {
>                 primary: {
>                     500: "#FF7000",
>                     100: "#FFF1E6",
>                 },
>                 dark: {
>                     100: "#000000",
>                     200: "#0F1117",
>                     300: "#151821",
>                     400: "#212734",
>                     500: "#101012",
>                 },
>                 light: {
>                     900: "#FFFFFF",
>                     800: "#F4F6F8",
>                     850: "#FDFDFD",
>                     700: "#DCE3F1",
>                     500: "#7B8EC8",
>                     400: "#858EAD",
>                 },
>                 "accent-blue": "#1DA1F2",
>             },
>             fontFamily: {
> 	            sans: ["var(--font-inter)"],
> 		        grotesk: ["var(--font-spaceGrotesk)"],
>                 inter: ["var(--fo ar(--font-spaceGrotesk)"],
>             },
>             boxShadow: {
>                 "light-100":
>                     "0px 12px 20px 0px rgba(184, 184, 184, 0.03), 0px 6px 12px 0px rgba(184, 184, 184, 0.02), 0px 2px 4px 0px rgba(184, 184, 184, 0.03)",
>                 "light-200": "10px 10px 20px 0px rgba(218, 213, 213, 0.10)",
>                 "light-300": "-10px 10px 20px 0px rgba(218, 213, 213, 0.10)",
>                 "dark-100": "0px 2px 10px 0px rgba(46, 52, 56, 0.10)",
>                 "dark-200": "2px 0px 20px 0px rgba(39, 36, 36, 0.04)",
>             },
>             backgroundImage: {
>                 "auth-dark": "url('/assets/images/auth-dark.png')",
>                 "auth-light": "url('/assets/images/auth-light.png')",
>             },
>             screens: {
>                 xs: "420px",
>             },
>             keyframes: {
>                 "accordion-down": {
>                     from: { height: "0px" },
>                     to: { height: "var(--radix-accordion-content-height)" },
>                 },
>                 "accordion-up": {
>                     from: { height: "var(--radix-accordion-content-height)" },
>                     to: { height: "0px" },
>                 },
>             },
>             animation: {
>                 "accordion-down": "accordion-down 0.2s ease-out",
>                 "accordion-up": "accordion-up 0.2s ease-out",
>             },
>         },
>     },
>     plugins: [require("tailwindcss-animate"), require("@tailwindcss/typography")],
> };
> export default config;
> ```

Also install these two plugins:
```bash 
npm install tailwindcss-animate @tailwindcss/typography
```
###### theme.css
Then we will be creating a special file `theme.css` within a `styles` folder. We want to optimize the process of writing our code. Many combinations of styles will often be reused.

Ex) The below code creates a new class `background-light-850_dark100` that will apply the specified background color based on active mode ( light or dark )
```css
@layer utilities{
    .background-light-850_dark100{
        @apply bg-light-850 dark:bg-dark-100;
    }
}
```

> [!file]+ styles/theme.css
> 
> ```css
> @tailwind base;
> @tailwind components;
> @tailwind utilities;
> 
> @layer utilities {
>   .background-light850_dark100 {
>     @apply bg-light-850 dark:bg-dark-100;
>   }
> 
>   .background-light900_dark200 {
>     @apply bg-light-900 dark:bg-dark-200;
>   }
> 
>   .background-light900_dark300 {
>     @apply bg-light-900 dark:bg-dark-300;
>   }
> 
>   .background-light800_darkgradient {
>     @apply bg-light-800 dark:dark-gradient;
>   }
> 
>   .background-light800_dark400 {
>     @apply bg-light-800 dark:bg-dark-400 !important;
>   }
> 
>   .background-light700_dark400 {
>     @apply bg-light-700 dark:bg-dark-400;
>   }
> 
>   .background-light700_dark300 {
>     @apply bg-light-700 dark:bg-dark-300;
>   }
> 
>   .background-light800_dark400 {
>     @apply bg-light-800 dark:bg-dark-400;
>   }
> 
>   .background-light800_dark300 {
>     @apply bg-light-800 dark:bg-dark-300 !important;
>   }
> 
>   .text-dark100_light900 {
>     @apply text-dark-100 dark:text-light-900 !important;
>   }
> 
>   .text-dark200_light900 {
>     @apply text-dark-200 dark:text-light-900;
>   }
> 
>   .text-dark200_light800 {
>     @apply text-dark-200 dark:text-light-800 !important;
>   }
> 
>   .text-dark300_light700 {
>     @apply text-dark-300 dark:text-light-700;
>   }
> 
>   .text-dark400_light700 {
>     @apply text-dark-400 dark:text-light-700;
>   }
> 
>   .text-dark500_light700 {
>     @apply text-dark-500 dark:text-light-700 !important;
>   }
> 
>   .text-dark500_light500 {
>     @apply text-dark-500 dark:text-light-500;
>   }
> 
>   .text-dark300_light900 {
>     @apply text-dark-300 dark:text-light-900 !important;
>   }
> 
>   .text-dark400_light800 {
>     @apply text-dark-400 dark:text-light-800;
>   }
> 
>   .text-light400_light500 {
>     @apply text-light-400 dark:text-light-500 !important;
>   }
> 
>   .text-dark400_light500 {
>     @apply text-dark-400 dark:text-light-500;
>   }
> 
>   .text-dark400_light900 {
>     @apply text-dark-400 dark:text-light-900 !important;
>   }
> 
>   .text-light400_light500 {
>     @apply text-light-400 dark:text-light-500 !important;
>   }
> 
>   .light-border {
>     @apply border-light-800 dark:border-dark-300;
>   }
> 
>   .light-border-2 {
>     @apply border-light-700 dark:border-dark-400 !important;
>   }
> 
>   .h1-bold {
>     @apply text-[30px] font-bold leading-[42px] tracking-tighter;
>   }
> 
>   .h2-bold {
>     @apply text-[24px] font-bold leading-[31.2px];
>   }
> 
>   .h2-semibold {
>     @apply text-[24px] font-semibold leading-[31.2px];
>   }
> 
>   .h3-bold {
>     @apply text-[20px] font-bold leading-[26px];
>   }
> 
>   .h3-semibold {
>     @apply text-[20px] font-semibold leading-[24.8px];
>   }
> 
>   .base-medium {
>     @apply text-[18px] font-medium leading-[25.2px];
>   }
> 
>   .base-semibold {
>     @apply text-[18px] font-semibold leading-[25.2px];
>   }
> 
>   .base-bold {
>     @apply text-[18px] font-bold leading-[140%];
>   }
> 
>   .paragraph-regular {
>     @apply text-[16px] font-normal leading-[22.4px];
>   }
> 
>   .paragraph-medium {
>     @apply text-[16px] font-medium leading-[22.4px];
>   }
> 
>   .paragraph-semibold {
>     @apply text-[16px] font-semibold leading-[20.8px];
>   }
> 
>   .body-regular {
>     @apply text-[14px] font-normal leading-[19.6px];
>   }
> 
>   .body-medium {
>     @apply text-[14px] font-medium leading-[18.2px];
>   }
> 
>   .body-semibold {
>     @apply text-[14px] font-semibold leading-[18.2px];
>   }
> 
>   .small-regular {
>     @apply text-[12px] font-normal leading-[15.6px];
>   }
> 
>   .small-medium {
>     @apply text-[12px] font-medium leading-[15.6px];
>   }
> 
>   .small-semibold {
>     @apply text-[12px] font-semibold leading-[15.6px];
>   }
> 
>   .subtle-medium {
>     @apply text-[10px] font-medium leading-[13px] !important;
>   }
> 
>   .subtle-regular {
>     @apply text-[10px] font-normal leading-[13px];
>   }
> 
>   .placeholder {
>     @apply placeholder:text-light-400 dark:placeholder:text-light-500;
>   }
> 
>   .invert-colors {
>     @apply invert dark:invert-0;
>   }
> 
>   .shadow-light100_dark100 {
>     @apply shadow-light-100 dark:shadow-dark-100;
>   }
> 
>   .shadow-light100_darknone {
>     @apply shadow-light-100 dark:shadow-none;
>   }
> 
>   .primary-gradient {
>     background: linear-gradient(129deg, #ff7000 0%, #e2995f 100%);
>   }
> 
>   .dark-gradient {
>     background: linear-gradient(
>       232deg,
>       rgba(23, 28, 35, 0.41) 0%,
>       rgba(19, 22, 28, 0.7) 100%
>     );
>   }
> }
> ```

###### globals.css
We will also recreate `globals.css` inside the `app` directory. Here we will set the default font for our app: 
```css
body {
    font-family: "Inter", sans-serif;
}
```
and add more utilitiy classes.

> [!file]+ app/globals.css
> ```css
> @tailwind base;
> @tailwind components;
> @tailwind utilities;
> 
> @import url("../styles/theme.css");
> 
> body {
>     font-family: "Inter", sans-serif;
> }
> 
> @layer utilities {
>     .flex-center {
>         @apply flex justify-center items-center;
>     }
> 
>     .flex-between {
>         @apply flex justify-between items-center;
>     }
> 
>     .flex-start {
>         @apply flex justify-start items-center;
>     }
> 
>     .card-wrapper {
>         @apply bg-light-900 dark:dark-gradient shadow-light-100 dark:shadow-dark-100;
>     }
> 
>     .btn {
>         @apply bg-light-800 dark:bg-dark-300 !important;
>     }
> 
>     .btn-secondary {
>         @apply bg-light-800 dark:bg-dark-400 !important;
>     }
> 
>     .btn-tertiary {
>         @apply bg-light-700 dark:bg-dark-300 !important;
>     }
> 
>     .markdown {
>         @apply max-w-full prose dark:prose-p:text-light-700 dark:prose-ol:text-light-700 dark:prose-ul:text-light-500 dark:prose-strong:text-white dark:prose-headings:text-white prose-headings:text-dark-400 prose-h1:text-dark-300 prose-h2:text-dark-300 prose-p:text-dark-500 prose-ul:text-dark-500 prose-ol:text-dark-500;
>     }
> 
>     .primary-gradient {
>         background: linear-gradient(129deg, #ff7000 0%, #e2995f 100%);
>     }
> 
>     .dark-gradient {
>         background: linear-gradient(232deg, rgba(23, 28, 35, 0.41) 0%, rgba(19, 22, 28, 0.7) 100%);
>     }
> 
>     .tab {
>         @apply min-h-full dark:bg-dark-400 bg-light-800 text-light-500 dark:data-[state=active]:bg-dark-300 data-[state=active]:bg-primary-100 data-[state=active]:text-primary-500 !important;
>     }
> }
> 
> .no-focus {
>     @apply focus-visible:ring-0 focus-visible:ring-transparent focus-visible:ring-offset-0 !important;
> }
> 
> .active-theme {
>     filter: invert(53%) sepia(98%) saturate(3332%) hue-rotate(0deg) brightness(104%) contrast(106%) !important;
> }
> 
> .light-gradient {
>     background: linear-gradient(
>         132deg,
>         rgba(247, 249, 255, 0.5) 0%,
>         rgba(229, 237, 255, 0.25) 100%
>     );
> }
> 
> .primary-text-gradient {
>     background: linear-gradient(129deg, #ff7000 0%, #e2995f 100%);
>     background-clip: text;
>     -webkit-background-clip: text;
>     -webkit-text-fill-color: transparent;
> }
> 
> .custom-scrollbar::-webkit-scrollbar {
>     width: 3px;
>     height: 3px;
>     border-radius: 2px;
> }
> 
> .custom-scrollbar::-webkit-scrollbar-track {
>     background: #ffffff;
> }
> 
> .custom-scrollbar::-webkit-scrollbar-thumb {
>     background: #888;
>     border-radius: 50px;
> }
> 
> .custom-scrollbar::-webkit-scrollbar-thumb:hover {
>     background: #555;
> }
> 
> /* Markdown Start */
> .markdown a {
>     color: #1da1f2;
> }
> 
> .markdown a,
> code {
>     /* These are technically the same, but use both */
>     overflow-wrap: break-word;
>     word-wrap: break-word;
> 
>     -ms-word-break: break-all;
>     /* This is the dangerous one in WebKit, as it breaks things wherever */
>     word-break: break-all;
>     /* Instead use this non-standard one: */
>     word-break: break-word;
> 
>     /* Adds a hyphen where the word breaks, if supported (No Blink) */
>     -ms-hyphens: auto;
>     -moz-hyphens: auto;
>     -webkit-hyphens: auto;
>     hyphens: auto;
> 
>     padding: 2px;
>     color: #ff7000 !important;
> }
> 
> .markdown pre {
>     display: grid;
>     width: 100%;
> }
> 
> .markdown pre code {
>     width: 100%;
>     display: block;
>     overflow-x: auto;
> 
>     color: inherit !important;
> }
> /* Markdown End */
> 
> /* Clerk */
> .cl-internal-b3fm6y {
>     background: linear-gradient(129deg, #ff7000 0%, #e2995f 100%) !important;
> }
> 
> .hash-span {
>     margin-top: -140px;
>     padding-bottom: 140px;
>     display: block;
> }
> 
> /* Hide scrollbar for Chrome, Safari and Opera */
> .no-scrollbar::-webkit-scrollbar {
>     display: none;
> }
> 
> /* Hide scrollbar for IE, Edge and Firefox */
> .no-scrollbar {
>     -ms-overflow-style: none; /* IE and Edge */
>     scrollbar-width: none; /* Firefox */
> }
> ```

###### ---

> [!tip]
> add this in `.vscode/settings.json` to hide tailwind warning lints.
> ```json
> {
>     "files.associations": {
>         "*.css": "tailwindcss"
>     }
> }
> ```

#### Setting the Icon, Font and Metadata
##### Icon
- Convert your icon into `.ico` format
- Rename your file to `favicon.ico`
- place this `favicon.ico` in the `app` folder

> [!info] If this doesn't work, try renaming `favicon.ico` to `icon.ico`

##### Metadata
We have to export a metadata object from  `layout.tsx` 
```tsx
import type { Metadata } from "next";

export const metadata: Metadata = {
    title: "CodeOverflow",
    description:
        "CodeOverflow is a programmer's Q&A platform inspired by Stack Overflow. Ask and answer questions, vote on the best solutions, and connect with a vibrant community of developers.",
};
```

##### Fonts
Import the Font function
```tsx
import { Inter, Space_Grotesk } from "next/font/google";
```

Create a font object with the font function. Here we will define the variable name and the different options for the font like which subset of the font to use, which weights etc.
```tsx
const inter = Inter({
    subsets: ["latin"],
    weight: ["100", "200", "300", "400", "500", "600", "700", "800", "900"],
    variable: "--font-inter",
});

const spaceGrotesk = Space_Grotesk({
    subsets: ["latin"],
    weight: ["300", "400", "500", "600", "700"],
    variable: "--font-spaceGrotesk",
});
```

And then just include the fonts using class names. ==Note that these font class names are defined through tailwind.== Make sure these are the same variables used in `tailwind.config.ts`
```ts
import type { Config } from "tailwindcss";

const config: Config = {
    ...,
    theme: {
	    ...,
        extend: {
            fontFamily: {
                sans: ["var(--font-inter)"],
                grotesk: ["var(--font-spaceGrotesk)"],
            },
            ...,
};
export default config;
```

Then we include the fonts in `<html>` tag.. and set the default font in the body using the class name defined in `tailwind.config.ts`. For example here:-
- `font-sans` for inter
- `font-grotesk` for space grotesk
```tsx
export default function RootLayout({ children }: { children: ReactNode }) {
    return (
		<html lang="en" className={`${inter.variable} ${spaceGrotesk.variable}`}>
			<body className="font-sans">
				This text will use default inter font taken from body className
				<h1 className="h1-bold font-grotesk">
					This text will use space grotesk font taken from this h1 className check for wrap
				</h1>
				<main>{children}</main>
			</body>
		</html>
    );
}
```

Just making these changes in the root `layout.tsx` file will ensure these fonts and metadata are used throughout our entire application.

#### Code Structure 
As applications grow, it is important for them to remain scalable. And one of the best ways to ==ensure scalability is proper file and folder structure== as it allows us to separate files, components and pages as our app grows.

These are the main folders to achieve this purpose in our app:

`app`: 
- Everything we will do in our app will happen within this folder especially the routing.

`components`:
- We will keep our smaller, reusable pieces of UI. 
- Eg) Cards, Forms etc.

`constants`:
-  For values that will be reused in our apps.
- Eg) A NavBar with different links. Each link should have a title, url and icon. We can store this as an array of objects.

`context`:
- For establishing our theme provider for dark and light mode.

`lib/actions`:  
- These actions will be our interaction with the back-end.

`database`: 
- For our database setup ( MongoDB in our case) 

`public`:
- For all our assets.

`styles`: 
- All the styles will go here.

`types`: 
- We will define our TypeScript types here.

We will also create a `.env.local` file for private keys that can't be pushed to GitHub. 

