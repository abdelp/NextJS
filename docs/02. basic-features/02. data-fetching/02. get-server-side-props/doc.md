# getServerSideProps

**What will happen if you export a function called `getServerSideProps`?**

If you export a function called `getServerSideProps` (Server-Side Rendering) from a page, Next.js will pre-render this page on each request using the data returned by `getServerSideProps`.

```js
export async function getServerSideProps(context) {
  return {
    props: {}, // will be passed to the page component as props
  }
}
```

## When does getServerSideProps run

`getServerSideProps` only runs on server-side and never runs on the browser.

If a page uses `getServerSideProps`, then:

- When you request this page directly, `getServerSideProps` runs at request time, and this page will be pre-rendered with the returned props

- When you request this page on client-side page transitions through `next/link` or `next/router`, Next.js sends an API request to the server, which runs `getServerSideProps`

**What does `getServerSideProps` return?**

`getServerSideProps` returns JSON which will be used to render the page.


**Where can `getServerSideProps` be exported from?**

`getServerSideProps` can only be exported from a **page**. You can't export it from non-page files.

Note that you must export `getServerSideProps` as a standalone function -- it will not work if you add `getServerSideProps` as a property of the page component.

## When should I use getServerSideProps

## getServerSideProps or API Routes

**Why is inefficient  to reach for an API Route when you want to fetch data from the server?**

It's unnecessary and inefficient approach, as it will cause an extra request to be made due to both `getServerSideProps` and API Routes running on the server. Instead, directly import the logic used inside your API Route into `getServerSideProps`.

## getServerSideProps with Edge API Routes

**Where can `getServerSideProps` be used?**

`getServerSideProps` can be used with both Serverless and Edge Runtimes, and you can set props in both.

**What's the difference between using `getServerSideProps` in Serverless and Edge Runtimes?**

In Edge Runtime, you do not have access to the response object.

**How can you set the runtime?**

You can explicitly set the runtime on a per-page basis by modifying the `config`, for example:

```js
export const config = {
  runtime: 'nodejs',
}
```

## Fetching data on the client side

## Using getServerSideProps to fetch data at request time

## Caching with Server-Side Rendering (SSR)

## Does getServerSideProps render an error page

**What will happen if an error is thrown inside `getServerSideProps`?**

If an error is thrown inside `getServerSideProps`, it will show the `pages/500.js` file.