# Testing standards

Scope: automated and manual tests. Rule IDs (`TEST-<n>`) are cited in plans, critiques and reviews. The rules come from the PWCC course: [Express](https://mihai-gheorghe.gitbook.io/pwcc-suport-curs/backend-in-javascript/express) ("Testarea API-ului") and [REST APIs](https://mihai-gheorghe.gitbook.io/pwcc-suport-curs/backend-in-javascript/api-rest) ("Testarea unui API din terminal").

## TEST-1. node:test, assert and fetch

Tests use the built-in `node:test` and `node:assert/strict`, in `test/*.test.js`. `npm test` runs `node --test`. No other test framework.

## TEST-2. HTTP tests start the app on a free port

```javascript
const { test, before, after } = require("node:test");
const assert = require("node:assert/strict");
const { once } = require("node:events");

let server;
let baseUrl;

before(async () => {
  server = createApp(/* test configuration */).listen(0); // port 0: the system picks a free port
  await once(server, "listening");
  baseUrl = `http://localhost:${server.address().port}`;
});

after(() => server.close());
```

## TEST-3. Essential tests only

For each requirement: the success path, and the error cases that protect security or data (401, 403, 409, invalid input that would corrupt data). No exhaustive input matrices; one test per behavior.

## TEST-4. Independent tests

Every test file uses its own temporary data (a temporary JSON file, a separate emulator project) and does not depend on the order of the tests. Test data never touches development or production data.

## TEST-5. No real external services

Firestore tests run on the Firestore emulator (`FIRESTORE_EMULATOR_HOST`). Tests do not call services on the Internet.

## TEST-6. Test names describe behavior

`"POST /auth/register with an e-mail already registered returns 409"`, not `"test 3"`.

## TEST-7. Existing tests keep passing

A task never deletes or weakens an existing test to make the suite pass. A test changes only when its requirement changed, and the plan says so.

## TEST-8. Manual tests are repeatable

Manual requests for an endpoint are kept in the repository, as curl commands in `README.md` or in an API collection (for example Postman), so that anyone can run them again.
