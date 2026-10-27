# Security standards

Scope: authentication, authorization, secrets and HTTP security. Rule IDs (`SEC-<n>`) are cited in plans, critiques and reviews. The rules come from the PWCC course: [Authentication and authorization](https://mihai-gheorghe.gitbook.io/pwcc-suport-curs/backend-in-javascript/autentificare), [Node.js](https://mihai-gheorghe.gitbook.io/pwcc-suport-curs/backend-in-javascript/nodejs) ("Variabilele de mediu") and [Securing the database](https://mihai-gheorghe.gitbook.io/pwcc-suport-curs/firebase/securizarea-bazei-de-date).

## SEC-1. Passwords are stored only as bcrypt hashes

`bcryptjs` with cost 10. A password is never stored in clear, logged or returned. bcrypt uses only the first 72 bytes of a password, so longer passwords are rejected at validation.

```javascript
const passwordHash = await bcrypt.hash(password, 10);
const matches = await bcrypt.compare(String(password), user.passwordHash);
```

## SEC-2. Login does not reveal which accounts exist

An unknown e-mail and a wrong password get the same 401 response and message. For an unknown e-mail, the password is still compared with a fixed dummy hash, so the response time does not differ either.

## SEC-3. JSON Web Tokens: HS256, short expiry, no secrets inside

`jwt.sign({ sub: user.id, email: user.email, role: user.role }, secret, { expiresIn: "1h" })`. The payload is signed, not encrypted: it never contains passwords or other secrets. Verification fixes the algorithm: `jwt.verify(token, secret, { algorithms: ["HS256"] })`.

## SEC-4. Authentication in a middleware

The token arrives in `Authorization: Bearer <token>`. An `authenticate` middleware verifies it and stores the payload in `req.user`. A missing, invalid or expired token gets 401.

## SEC-5. Authorization after authentication

Authorization rules (roles, ownership of a resource) run after `authenticate` and answer 403 when the user is known but not allowed. A role check is a middleware factory, `requireRole("admin")`. A user reads and changes only the resources they own.

## SEC-6. Secrets come from the environment

`JWT_SECRET` and every key come from environment variables, in development from `.env` (`node --env-file=.env`). `.env`, key files and data files are in `.gitignore`. The app refuses to start without `JWT_SECRET`. A strong secret: `node -e "console.log(require('node:crypto').randomBytes(32).toString('base64url'))"`.

## SEC-7. Security headers and rate limiting

`helmet()` on every response. `express-rate-limit` on `/auth/login`: at most 5 failed attempts per IP address in 15 minutes (`skipSuccessfulRequests: true`), then 429 in the standard error format.

## SEC-8. CORS only for known origins

`cors()` without options only in development; in production, the allowed origin is named explicitly.

## SEC-9. Input is checked against what is allowed

Validation accepts known-good shapes (allow-lists, lengths, formats) instead of removing "dangerous" characters. Never trust a value only because the client checked it.

## SEC-10. No secrets in logs; dependencies checked

Logs never contain passwords, tokens or keys. `npm audit` runs before a release.

## SEC-11. The server enforces authorization for Firestore

The Admin SDK bypasses Firestore security rules, so every authorization check stays in the server code. The service account key never reaches a commit or a browser.
