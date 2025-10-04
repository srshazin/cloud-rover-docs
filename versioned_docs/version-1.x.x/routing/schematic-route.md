---
sidebar_position: 3
title: Schematic Routes
---

Starting with version `1.0.13` we added a type of routes called schematic routes. A schematic or wildcard path is kind a like a templated path that will accept anything as a subroute.

## Explanation
Suppose your route path is defined as `/user`. It's called an absolute path. Meaning request must match to `/user` to be considered as a valid path otherwise, request will hit a 404 response. We have another kind of route called dynamic route so you can pass a path like `/user/{id}` then a request to `/user/123` is valid and you get the id via the `params` interface. But it is limited to a single level of  `/`. 

The **schematic path** will be very useful when you need a full control over a url schema. For instance you define a path like `/proxy/*` and you get full control over the route i.e anything comes in place of the asterisk `(*)`. So suppose your requested path is `/proxy/xyz` this request will still hit the handler you defined for the path `/proxy/*` and from inside the handler you can actually read the requested **subpath** i.e the path in place of the `*`. We're calling it a subpath.


## Defining a Schematic Route
Defining a schematic route is simple just like any other type of routes. Here we only need to explicitly specify it's a schematic path setting `isSchematic` to `true`. Here's an example

```ts
{
    path: '/foo/*',
    method: 'GET',
    handler: handleSchematicRoute,
    isSchematic: true,
}
```

That's all. Now if incoming request hits anything after `/foo` like `/foo/bar/xyz` you can still handle those request using the same handler function.

:::warning
Currently the wildcard can only be placed at the end of the path and with a leading slash.
**For example**
- `/foo/bar/*` (Will Work ✅)
- `/foo/*/bar` (Won't Work ❌)
- `/foo/bar*` (Won't Work ❌)
:::

## Getting the Subpath
So let's get back to the previous example.
```plaintext
Defined schematic: path: /foo/*
Requested path: /foo/bar/xyz
```

With the `v1.0.13` we've added a new property to the Route Context or `RC` interface. And you guessed it it's called `subPath`. So it's directly accessible from inside the handler function. 

### Example

```ts
async function handleSchematicRoute({ request, subPath }: RC): Promise<Response> {
	return reply.text(`Incoming request at subpath ${subPath}`);
}
```

Now you can do whatever you want with that path. It can be useful when you are building a reverse-proxy. 