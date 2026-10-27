# JavaScript standards

Scope: all JavaScript in `src/` and `test/`. Rule IDs (`JS-<n>`) are cited in plans, critiques and reviews. The rules come from the PWCC course: [JavaScript basics](https://mihai-gheorghe.gitbook.io/pwcc-suport-curs/notiuni-de-baza-javascript/descriere-limbaj) ("Bune practici"), [Asynchronous JavaScript](https://mihai-gheorghe.gitbook.io/pwcc-suport-curs/javascript-avansat/javascript-asincron), [Functional programming](https://mihai-gheorghe.gitbook.io/pwcc-suport-curs/javascript-avansat/programare-functionala) and [Node.js](https://mihai-gheorghe.gitbook.io/pwcc-suport-curs/backend-in-javascript/nodejs).

## JS-1. Names say what the code does, in English

camelCase for variables and functions, UPPER_SNAKE_CASE for fixed constants, PascalCase for classes. Names describe the purpose, not the type.

```javascript
const MAX_TITLE_LENGTH = 200;
function findTaskById(tasks, id) {}
class TaskStore {}
```

## JS-2. `const` by default

`let` only for variables that get a new value, `var` never. A reader sees at once what can change.

## JS-3. Strict comparisons and explicit conversions

`===` and `!==`, never `==` and `!=`. Convert with `Number()` and `String()` where a type is needed: route parameters and query strings are strings.

## JS-4. Small functions with one responsibility

A function that validates input, saves data and builds the response does three things. Split it: each part can be understood, tested and reused alone.

## JS-5. Comments explain why, not what

```javascript
// Same message for an unknown e-mail and a wrong password: the response must not reveal which accounts exist.
```

## JS-6. Modules, not globals

CommonJS modules (`require`, `module.exports`). Each file exports only what other files use. Built-in modules are imported with the `node:` prefix: `require("node:fs/promises")`.

## JS-7. Asynchronous code with `async` and `await`

Errors from awaited promises are caught with `try`/`catch` where the code can recover; otherwise they propagate (Express 5 sends them to the error handler, see `API-6`). Independent operations start together and are awaited with `Promise.all()`.

## JS-8. Pure logic, side effects at the edges

Validation and data transformations are pure functions: no changes to their arguments or to outside state, the same result for the same input. Reading files, writing data and sending responses stay at the edges (routes, storage). Copies are made with the spread operator instead of changing the original.

```javascript
function withStatus(task, status) {
  return { ...task, status, updatedAt: new Date().toISOString() };
}
```

## JS-9. Missing values with `??` and `?.`

`??` gives a default only for `null` and `undefined`; `?.` reads a property of a value that may be missing. A request body is read as `req.body ?? {}`.

## JS-10. Linting passes

`npm run lint` (ESLint, recommended rules plus `eqeqeq`, `prefer-const` and `no-var`) reports no problems. Generated code goes through the same checks as handwritten code.
