# Dynamic Routes

**What will happen if query params matches route parameters?**

Route parameters will override query parameters with the same name. For example, the route `/post/abc?pid=123` will have the following `query` object:

```js
{ "pid": "abc" }
```

**How are client-side navigations to dynamic routes handled?**

Client-side navigations to dynamic routes are handled with `next/link`.

## Catch all routes

**How can dynamic routes be extended to catch all paths?**

Dynamic routes can be extended to cath all paths by adding three dots (`...`) inside the brackets. For example:

- `pages/post/[...slug].js` matches `/post/a`, but also `/post/a/b`, `/post/a/b/c` and so on.

**Where will the matched params be sent?**

Matched params will be sent as a query param to the page, and it will always be an array.

**Example:**

path `/post/a` will have the following `query` object:

```js
{ "slug": ["a"] }
```

And in the case of `/post/a/b`, and any other matching path, new parameters will be added to the array, like so:

```js
{ "slug": ["a", "b"] }
```

## Optional catch all routes

**How can catch all parameters be made optional?**

Catch all routes can be made optional by including the parameter in double brackets (`[[...slug]]`).

For example, `pages/post/[[...slug]].js` will match `/post`, `/post/a`, `/post/a/b`, and so on.

## Caveats

* Predefined routes take precedence over dynamic routes.
* Dynamic routes take precedence over catch all routes.

- Pages that are statically optimized by Automatic Static Optimization will be hydrated without their route parameters provided, i.e. `query` will be an empty object (`{}`).

After hydration, Next.js will trigger an update to your application to provide the route parameters in the `query` object.

