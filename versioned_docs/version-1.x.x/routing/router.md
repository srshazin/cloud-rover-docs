---
sidebar_position: 1
title: Router
---

Rover middleware needs a router to operate. Router is essentially an array of `Route` object.

## The route interface

The route object looks somewhat like this:

```ts
export interface Route {
  path: string;
  handler(rc: RC): Promise<Response>;
  method?:
    | "GET"
    | "POST"
    | "PUT"
    | "DELETE"
    | "PATCH"
    | "HEAD"
    | "OPTIONS"
    | "CONNECT"
    | "*";
  isSchematic?: boolean;
}
```

Here,
- `path` is the url path which starts with a `/` for instance `/blog`
- `handler` is the handler function which accepts `rc` as a param which is useful for inspecting requests properties. More about it is described in the [Handler Function](../handler-function.md) section. 
- `method` as the name suggests the http method like `POST` or `GET`. By default the method will be `GET`, if it's not explicitly defined.

## Create Router

Creating router is simple, just call the built-in `Router` function and pass in an array of `Route`

For instance here's a demo router

**router.js**

```js
const router = Router([
  {
    path: "/",
    handler: home_handler,
    method: "GET",
  },
  {
    path: "/blogs",
    handler: blog_handler,
    method: "GET",
  },
  {
    path: "/auth/login",
    handler: login_handler,
    method: "POST",
  },
]);
```

That's it, it's as simple as that. No need to make it any complex further. This design helps you keep a separate route tree for your application.

## Wildcard (*) Method
If you look at the method types you can see at the bottom there's a method type of `*` apart from the standard http methods. With the version `1.0.13` we've introduced a new type of method and we're calling it wildcard method. The idea is simple, this method will allow any incoming request of any method that's all you need to know. 