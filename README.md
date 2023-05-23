# Next.js
- Next.js is a React framework that gives you building blocks to create web applications.
- Next.js handles the tooling and configuration needed for React, and provides additional structure, features, and optimizations for your application.

<p align='center'>
    <img src="/.images/nextjs.png" height=300>
</p>

Next.js aims to have best-in-class developer experience and many built-in features, such as:
- An intuitive page-based routing system (with support for dynamic routes)
- Pre-rendering, both static generation (SSG) and server-side rendering (SSR) are supported on a per-page basis
- Automatic code splitting for faster page loads
- Client-side routing with optimized prefetching
- Built-in CSS and Sass support, and support for any CSS-in-JS library
- Development environment with Fast Refresh support
- API routes to build API endpoints with Serverless Functions
- Fully extendable

### Client-Side Navigation
- The Link component enables client-side navigation between two pages in the same Next.js app.
- Client-side navigation means that the page transition happens using JavaScript, which is faster than the default navigation done by the browser.

```
import Link from 'next/link';

Read <Link href="/posts/first-post">this page</Link>
```

### Code splitting and prefetching
- Next.js does code splitting automatically, so each page only loads what’s necessary for that page. 
  - That means when the homepage is rendered, the code for other pages is not served initially.
  - This ensures that the homepage loads quickly even if you have hundreds of pages.
- Only loading the code for the page you request also means that pages become isolated. If a certain page throws an error, the rest of the application would still work.
- Furthermore, in a production build of Next.js, whenever Link components appear in the browser’s viewport, Next.js automatically prefetches the code for the linked page in the background. 
- By the time you click the link, the code for the destination page will already be loaded in the background, and the page transition will be near-instant!

### Image Component and Image Optimization
- <i>next/image</i> is an extension of the HTML <img> element, evolved for the modern web.
- Next.js also has support for Image Optimization by default. 
  - Images are lazy loaded by default. 
  - This allows for resizing, optimizing, and serving images in modern formats like WebP when the browser supports it. 
  - This avoids shipping large images to devices with a smaller viewport. It also allows Next.js to automatically adopt future image formats and serve them to browsers that support those formats.
- Automatic Image Optimization works with any image source. Even if the image is hosted by an external data source, like a CMS, it can still be optimized.

### Pre-rendering:
- By default, Next.js pre-renders every page. This means that Next.js generates HTML for each page in advance, instead of having it all done by client-side JavaScript. 
- Pre-rendering can result in better performance and SEO.
- Each generated HTML is associated with minimal JavaScript code necessary for that page. 
- When a page is loaded by the browser, its JavaScript code runs and makes the page fully interactive. (This process is called hydration.)
	
	
#### Q: How to check That Pre-rendering Is Happening
You can check that pre-rendering is happening by taking the following steps:
1. Disable JavaScript in your browser. (Here’s how in Chrome).
2. Try accessing this page (the final result of this tutorial).
	
	
### Two Forms of Pre-rendering
Next.js has two forms of pre-rendering: Static Generation and Server-side Rendering. The difference is in when it generates the HTML for a page.
- Static Generation is the pre-rendering method that generates the HTML at build time. The pre-rendered HTML is then reused on each request.
- Server-side Rendering is the pre-rendering method that generates the HTML on each request.

#### Per-page Basis
- Importantly, Next.js lets you choose which pre-rendering form to use for each page. 
- You can create a "hybrid" Next.js app by using Static Generation for most pages and using Server-side Rendering for others.

#### When to Use Static Generation v.s. Server-side Rendering
- You should ask yourself: "Can I pre-render this page ahead of a user's request?" If the answer is yes, then you should choose Static Generation.
- On the other hand, Static Generation is not a good idea if you cannot pre-render a page ahead of a user's request. Maybe your page shows frequently updated data, and the page content changes on every request.
- In that case, you can use Server-side Rendering. It will be slower, but the pre-rendered page will always be up-to-date. Or you can skip pre-rendering and use client-side JavaScript to populate frequently updated data.

### Static Generation with Data
- Data Fetch at Build time
- in Next.js, when you export a page component, you can also export an async function called getStaticProps. If you do this, then:
  - getStaticProps runs at build time in production, and…
  - Inside the function, you can fetch external data and send it as props to the page.

```
export default function Home( props ) {...}

export async function getStaticProps(){
	// Get external data from the file system, API, DB, etc.
	const data = …
	
	// The value of the `props` key will be
	//  passed to the `Home` component
	
	return { props:...} }
```	
	
#### Development vs. Production
- In development (npm run dev or yarn dev), <i>getStaticProps</i> runs on every request.
- In production, getStaticProps runs at build time. However, this behavior can be enhanced using the fallback key returned by getStaticPaths
- Because it’s meant to be run at build time, you won’t be able to use data that’s only available during request time, such as query parameters or HTTP headers.


### Server-side rendering:
- To use Server-side Rendering, you need to export <i>getServerSideProps</i> instead of <i>getStaticProps</i> from your page.
- Because getServerSideProps is called at request time, its parameter (context) contains request specific parameters.

### Client-side Rendering
If you do not need to pre-render the data, you can also use the following strategy (called Client-side Rendering):
- Statically generate (pre-render) parts of the page that do not require external data.
- When the page loads, fetch external data from the client using JavaScript and populate the remaining parts.

#### SWR:
- The team behind Next.js has created a React hook for data fetching called SWR. We highly recommend it if you’re fetching data on the client side. 
- It handles caching, revalidation, focus tracking, refetching on interval, and more. We won’t cover the details here, but here’s an example usage:

### Dynamic Routes
- Pages that begin with [ and end with ] are dynamic routes in Next.js.
  - Ex: [id].js
- We’ll export an async function called <i>getStaticPaths</i> from this page. 
  - In this function, we need to return a list of possible values for id.
- Finally, we need to implement <i>getStaticProps</i> again - this time, to fetch necessary data for the blog post with a given id. 
  - getStaticProps is given params, which contains id (because the file name is [id].js).

### API Routes
- API Routes let you create an API endpoint inside a Next.js app. 
- You can do so by creating a function inside the pages/api directory that has the following format:

```
export default function handler(req,res){
    res.status(200).json({text:'Hello'});
}
```
