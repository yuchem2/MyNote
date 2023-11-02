# Installation

System Requirements:
- Node.js 18.17 or later
- macOS, Windows (including WSL), and Linux are supported

## Automactic installation
using `create-next-app`, which sets up everything automatically. 
```shell
npx create-next-app
```

On installation, can see the following prompts:
```shell
√ What is your project named? ... my-test-app
√ Would you like to use TypeScript? ... No / Yes
√ Would you like to use ESLint? ... No / Yes
√ Would you like to use Tailwind CSS? ... No / Yes
√ Would you like to use `src/` directory? ... No / Yes
√ Would you like to use App Router? (recommended) ... No / Yes
√ Would you like to customize the default import alias (@/*)? ... No / Yes
```

After the prompts, `create-next-app` will create a folder with your porject name and install the required dependencies

```ad-check
title: Good to know
[[Next.js]] now ships with [[TypeScript]], [[ESLint]], [[Tailwind CSS]] configuration by defualt.
Can opionally use a `src` directory in the root of poject to separate application's code from configuration files
```

## Manual installation
create a new [[Next.js]] app, install the required packages:
```shell
npm install next@latest react@latest react-dom@latest
```
Open `packge.json` file and add the following `scripts`
```json
{
	"scripts":{
		"dev": "next dev",
		"build": "next build",
		"start": "next start",
		"lint": "next lint"
	}
}
```
+ `dev` runs `next dev` to start Next.js in development mode
+ `build` runs `next build` to build the application for production usage
+ `start` runs `next start` to start a Next.js production server
+ `lint` runs `next lint` to set up Next.js built-in ESLint configuration

### Create Directories
Next.js uses file-system routing, which means the routes in application are determined by how structe files. 

#### The `app` directory
For new applciations, recommend using the App Router. This router allows to use React's latest features an dis an evolution of the Pages Router based on community feedback.

Create `app/` folder, then add a `layout.tsx` and `page.tsx` file. These will be rendered when the user visits the root of your application (`/`)

Create a root layout inside `app/layout.tsx` with the required `<html>` and `<body>` tags:
```typescript
export default function RootLayout( {
	children,
}: {
	children: React.ReactNode
}) {
	reutrn (
		<html lang="en">
			<body>{children}</body>
		</html>
	)
}
```
Finally, create a home page `app/page.tsx` with some initial content:
```TypeScript
export defualt function Page() {
	return <h1>Hello, Next.js!</h1>
}
```
If forget to create `layout.tsx`, Next.js will automatically create this file when running development server with `next dev`

#### The `page` directory (optional)
If prefer to use Pages Router instead of the App Router, you can create a `pages/` dircetory at the root of your project.

Then, add an `index.tsx` file inside `pages` folder. This will be home page(`/`)
```TypeScript
export default function Page() {
	return <h1>Hello, Next.js!</h1>
}
```
Next, add ad `_app.tsx` file inside `pages/` to define the global layout. 
```typescript
import type { AppProps } from 'next/app'

export default function App ({ Component, pageProps }: AppProps) {
	return <Component {...pageProps} />
}
```
Finally, add a `_document.tsx` file inside `pages/` to control the initial response from the server.Learn more about the custom Document file.
```typescript
import { Html, Head, Main, NextSCript } form `next/document`

export default function Document () {
	return (
		<Html>
			<Head />
			<body>
				<Main />
				<NextScript />
			</body>
		</Html>
	)
}
```

```ad-check
title: Good to know
Although can use both routers in the same project, routes in `app` will be prioritized over `pages`. Recommend using only one router in new project to avoid confusion.
```

#### The `public` folder (optional)
Create a `public` folder to store static assets such as images, fonts, etc. Files inside `public` directory can then be refrerenced by code starting from the base URL(`/`)

## Run the Development Server
1. Run `npm run dev` to start the development server.
2. Visit `http://localhost:3000` to view application
3. Edit `app/layout.tsx` (or `pages/index.tsx`) file and save it to see the updated result in browser.