# REST API standards

Scope: the HTTP interface, in `src/`. Rule IDs (`API-<n>`) are cited in plans, critiques and reviews. The rules come from the PWCC course: [Express](https://mihai-gheorghe.gitbook.io/pwcc-suport-curs/backend-in-javascript/express) and [REST APIs](https://mihai-gheorghe.gitbook.io/pwcc-suport-curs/backend-in-javascript/api-rest).

## API-1. Resources are plural nouns

A collection and its items: `/tasks`, `/tasks/:id`. No verbs in paths (`/getTasks`, `/createTask`). Authentication actions are the exception: `/auth/register`, `/auth/login`.

## API-2. HTTP methods by their meaning

GET reads, POST creates, PATCH changes the fields it receives, DELETE removes. GET never changes data.

## API-3. Status codes

| Code | When |
| --- | --- |
| 200 | a successful read or update, with the resource in the body |
| 201 | a resource was created; the `Location` header holds its address |
| 204 | a successful delete, no body |
| 400 | invalid JSON or data that fails validation |
| 401 | missing, invalid or expired credentials |
| 403 | the user is known but not allowed |
| 404 | the resource or the route does not exist |
| 409 | conflict with existing data, for example an e-mail already registered |
| 429 | too many requests |
| 500 | an unexpected error on the server |

## API-4. One error format

Every error response has the same JSON shape: `{ "error": "<message>" }`. Messages are short and safe to show to the user.

## API-5. Validate every field on the server

Client-side checks can be bypassed (curl, Postman). Each field is checked for presence, type, length and allowed values before any data changes; unknown fields are ignored.

```javascript
const { title, description = "" } = req.body ?? {};
if (typeof title !== "string" || title.trim() === "") {
  return res.status(400).json({ error: "title is required" });
}
```

## API-6. A 404 handler for unknown routes and one error handler

After the routes: a middleware that answers 404 for unknown routes, then an error middleware with four parameters. It uses `error.status` when set (400 for invalid JSON) and answers 500 otherwise, without the stack trace, which goes only to the server log.

```javascript
app.use((req, res) => {
  res.status(404).json({ error: `Route ${req.method} ${req.path} does not exist` });
});

app.use((error, req, res, next) => {
  const status = error.status ?? 500;
  if (status >= 500) console.error(error);
  res.status(status).json({ error: status >= 500 ? "Internal server error" : error.message });
});
```

## API-7. The app is built separately from the server

A function builds and returns the Express app without listening; a separate entry point reads the configuration and calls `listen()`. Tests import the app and start it on a free port.

## API-8. JSON in camelCase

Field names in requests and responses use camelCase (`createdAt`, `dueDate`). Dates are ISO 8601 strings.

## API-9. Responses never contain secrets

Password hashes, tokens of other users and internal fields are removed before an object is sent.
