# Data standards

Scope: persistence, in a local JSON file and in Cloud Firestore. Rule IDs (`DATA-<n>`) are cited in plans, critiques and reviews. The rules come from the PWCC course: [Google Cloud Firestore](https://mihai-gheorghe.gitbook.io/pwcc-suport-curs/firebase/cloud-firestore) and [CRUD operations](https://mihai-gheorghe.gitbook.io/pwcc-suport-curs/firebase/operatii-crud).

## DATA-1. All persistence goes through the storage module

Routes and business logic call storage functions; they never read files or call Firestore directly. Changing the storage (JSON file, then Firestore) does not change them.

## DATA-2. Storage functions are asynchronous and return plain objects

Every storage function returns a promise. Records are plain objects with a string `id`, so the JSON file and Firestore return the same shapes.

## DATA-3. The JSON file is written safely

The file path comes from configuration (for example `DATA_FILE`), so tests use a temporary file. A write goes to a temporary file first, which then replaces the data file (`rename`), so a crash never leaves a half-written file. Writes from one process are serialized.

## DATA-4. Firestore through one module

One module initializes the Admin SDK once and exports the database; the rest of the app imports it. Credentials come from `GOOGLE_APPLICATION_CREDENTIALS`; the emulator is selected with `FIRESTORE_EMULATOR_HOST`.

## DATA-5. Check-then-write runs as one operation

A rule like "the e-mail must be unique" is checked and written in the same operation: a Firestore transaction, or a single serialized read-modify-write of the JSON file. Otherwise two simultaneous requests can both pass the check.

## DATA-6. Updates change only the given fields

A partial update changes the fields received and keeps the others; updating a record that does not exist is an error (404 in the API).

## DATA-7. Seed and test data are separate from real data

Generated data (seed scripts, test fixtures) goes to its own file or emulator project and is never mixed with real data.
