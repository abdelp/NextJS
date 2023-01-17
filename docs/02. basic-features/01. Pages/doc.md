# Pages

## Pages with Dynamic Routes

If you create a file called `pages/posts/[id].js`, then it will be accessible at `posts/1`, `posts/2`, etc.

## Two forms of Pre-rendering

**Which are the two types that Next.js has for pre-rendering?**

- **Static Generation (Recommended):** The HTML is generated at **build time** and will be reused on each request.
- **Server-side Rendering:** The HTML is generated on **each request**.

**Which is the recommended way to pre-render?**

They **recommend** using **Static Generation** over Server-side Rendering for performance reasons.

## Static Generation

### Static Generation without data

### Static Generation with data

Two scenarios where pages require fetching external data for pre-rendering:

1. Your page **content** depends on external data: Use `getStaticProps`.
2. Your page **paths** depend on external data: Use `getStaticPaths` (usually in addition to `getStaticProps`).

#### Scenario 1: Your page content depends on external data

**Example:**

**What does Next.js allow to fetch data on pre-render?**

To fetch this data on pre-render, Next.js allows you to `export` an `async` function called `getStaticProps` from the same file.

**When is getStaticProps function getting called?**

This function gets called at build time and lets you pass fetched data to the page's `props` on pre-render.

```js
export default function blog({ posts }) {
  // Render posts...
}

// This function gets called at build time
export async function getStaticProps() {
  // Call an external API endpoint to get posts
  const res = await fetch('https://.../posts')
  const posts = await res.json()

  // By returning { props: { posts } }, the Blog component
  // will receive `posts` as a prop at build time
  return {
    props: {
      posts,
    },
  }
}
```

#### Scenario 2: Your page paths depend on external data

**What does Next.js offer to handle pre-render paths that depend on external data?**

Next.js lets you `export` an `async` function called `getStaticPaths` from a dynamic page.

**When is getStaticPaths getting called?**

This function gets called at build time and lets you specify which paths you want to pre-render.

```js
// This function gets called at build time
export async function getStaticPaths() {
  // Call an external API endpoint to get posts
  const res = await fetch('https://.../posts')
  const posts = await res.json()

  // Get the paths we want to pre-render based on posts
  const paths = posts.map((posts) => ({
    params: { id: post.id },
  }))

  // We'll pre-render only these paths at build time.
  // { fallback: false } means other routes should 404.

  return { paths, fallback: false }
}
```

Also in `pages/posts/[id].js`, you need to export `getStaticProps` so that you can fetch the data about the post with this `id` and use it to pre-render the page:

```js
export default function Post({ post }) {
  // Render post...
}

export async function getStaticPaths() {
  // ...
}

// This also gets called at build time
export async function getStaticProps({ params }) {
  // params contains the post `id`.
  // If the route is like /posts/1, then params.id is 1
  const res = await fetch(`https://.../posts/${params.id}`)
  const post = await res.json()

  // Pass post data to the page via props
  return { props: { post } }
}
```

## When should I use Static Generation?

It's recommended to use **Static Generation** (with and without data) whenever possible because your page can be built once and served by CDN, which makes it much faster than having a server render the page on every request.

**What you can when you can't pre-render static pages?**

- Use Static Generation with **Client-side data fetching:** You can skip pre-rendering some parts of a page and then use client-side JavaScript to populate them.
- Use **Server-Side Rendering:** Next.js pre-renders a page on each request.

## Server-side Rendering

`
Also referred to as "SSR" or "Dynamic Rendering"
`

**When is the page rendered if it's using Server-side Rendering?**

If a page uses **Server-side Rendering**, the page HTML is generated on **each request**.

**What it needs to be done to use Server-side rendering for a page?**

To use Server-side Rendering for a page, you need to `export` an `async` function called `getServerSideProps`.

**When will getServerSideProps get called?**

`getServerSideProps` will be called by the server on every request.

**Example:**

```js
export default function Page({ data }) {
  // Render data...
}

// This gets called on every request
export async function getServerSideProps() {
  // Fetch data from external API
  const res = await fetch('https://.../data')
  const data = await res.json()

  // Pass data to the page via props
  return { props: { data } }
}
```

## Summary