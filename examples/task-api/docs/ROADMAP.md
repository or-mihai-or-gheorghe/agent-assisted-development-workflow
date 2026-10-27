# Roadmap

Each item is one increment, developed as a task (see `AGENTS.md`). Status: planned or done.

| # | Slug | Increment | Status | Pull request |
| --- | --- | --- | --- | --- |
| 1 | app-foundation | Express app and server, configuration from environment variables, request logging, health check, JSON responses for unknown routes and errors, lint and test setup | planned | |
| 2 | user-registration | `POST /auth/register`: e-mail and password, password hashing, unique e-mail; users in a local JSON file; manual testing with Postman or a similar client | planned | |
| 3 | user-login | `POST /auth/login` with input validation; issues a JSON Web Token | planned | |
| 4 | controllers-services | Restructure the code into routes, controllers and services, with no change in behavior | planned | |
| 5 | task-crud | CRUD on `/tasks`, without authentication | planned | |
| 6 | dummy-data | Seed script with realistic fake users and tasks (Faker or similar), for testing with more content | planned | |
| 7 | task-authorization | `/tasks` only for authenticated users; each user manages only their own tasks; stricter validation | planned | |
| 8 | firestore-storage | Cloud Firestore behind the storage module; tests on the Firestore emulator | planned | |

## Follow-ups

Items found while working on other tasks, not yet scheduled.

| Slug | Description | Found in |
| --- | --- | --- |
| None | | |
