# Architecture

## Overview

A stateless REST API: Node.js 22 or newer, Express 5, CommonJS modules. Clients exchange JSON over HTTP; authenticated requests carry a JSON Web Token in the `Authorization` header. No code exists yet; the components appear with the increments in `docs/ROADMAP.md`.

## Components

| Component | Location | Responsibility |
| --- | --- | --- |
| None yet | | |

## Data model

| Entity | Fields | Stored in |
| --- | --- | --- |
| None yet | | |

## Request flow

To be described by the first increment.

## Decisions

| ID | Decision | Reason | Task |
| --- | --- | --- | --- |
| D1 | Node.js 22 or newer, Express 5, CommonJS modules | the stack of the PWCC course; Express 5 sends rejected promises from async routes to the error handler | before the first task |
| D2 | All persistence behind one storage module: a local JSON file first, Cloud Firestore later | the switch to Firestore must not change routes or business logic (`DATA-1`) | before the first task |
| D3 | Stateless authentication with JSON Web Tokens in the `Authorization` header | REST statelessness (`SEC-3`, `SEC-4`) | before the first task |
| D4 | Tests with `node:test` and `fetch` against the app started on a free port | no extra test framework (`TEST-1`, `TEST-2`) | before the first task |
