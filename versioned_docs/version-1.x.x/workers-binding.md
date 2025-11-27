---
sidebar_position: 6
title: Manage Worker Bindings
---

In order to work with runtime APIs called bindings, you can follow cloudflare's [official docs](https://developers.cloudflare.com/workers/runtime-apis/bindings/) for them. In this section however we'll discuss how you can use those bindings easily from rover.

## Using Binding from Rover

Suppose you have a worker binding like this

```jsonc
"d1_databases": [
		{
			"binding": "DB",
			"database_name": "db-name",
			"database_id": "<db-id>"
		}
	],

```

You can access it from inside a handler by using `rc.env.DB`

```js
export async function addPost(rc) {
	const body = (await request.json()) as RequestModel;
	const DB = rc.env.db
}
...
```

For type-safety in typescript you can define a custom type and expose it like this

```ts
// Custom Env type
export interface Env {
	DB: D1Database;
}

// A utility function to extract DB
export function extractEnv(env: unknown): Env {
	return env as Env;
}

export async function addPost({ request, env }: RC): Promise<Response> {
	const body = (await request.json()) as RequestModel;
	const { DB } = extractEnv(env);
  ...
}
```
