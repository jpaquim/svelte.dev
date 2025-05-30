---
title: The <form> element
---

In the chapter on [loading data](page-data), we saw how to get data from the server to the browser. Sometimes you need to send data in the opposite direction, and that's where `<form>` — the web platform's way of submitting data — comes in.

Let's build a todo app. We've already got an in-memory database set up in `src/lib/server/database.js`, and our `load` function in `src/routes/+page.server.js` uses the [`cookies`](/docs/kit/load#Cookies) API so that we can have a per-user todo list, but we need to add a `<form>` to create new todos:

```svelte
/// file: src/routes/+page.svelte
<h1>todos</h1>

+++<form method="POST">
	<label>
		add a todo:
		<input
			name="description"
			autocomplete="off"
		/>
	</label>
</form>+++

<ul class="todos">
```

If we type something into the `<input>` and hit Enter, the browser makes a POST request (because of the `method="POST"` attribute) to the current page. But that results in an error, because we haven't created a server-side _action_ to handle the POST request. Let's do that now:

```js
/// file: src/routes/+page.server.js
import * as db from '$lib/server/database.js';

export function load({ cookies }) {
	// ...
}

+++export const actions = {
	default: async ({ cookies, request }) => {
		const data = await request.formData();
		db.createTodo(cookies.get('userid'), data.get('description'));
	}
};+++
```

The `request` is a standard [Request](https://developer.mozilla.org/en-US/docs/Web/API/Request) object; `await request.formData()` returns a [`FormData`](https://developer.mozilla.org/en-US/docs/Web/API/FormData) instance.

When we hit Enter, the database is updated and the page reloads with the new data.

Notice that we haven't had to write any `fetch` code or anything like that — data updates automatically. And because we're using a `<form>` element, this app would work even if JavaScript was disabled or unavailable.
