# Browser Fallback for GitHub Publishing

Use this reference only when the GitHub CLI is unavailable or unauthenticated, or when the user explicitly chooses the browser workflow.

## Authentication

1. Open `https://github.com/new` in the Codex in-app browser.
2. If the page redirects to login, make the tab visible, mark it deliverable if it must survive the turn, and ask the user to sign in. Never request or handle their password.
3. Confirm the signed-in account and never proceed under an unexpected account.

## Create the Repository

1. Verify the repository owner, name, and Public/Private selection before clicking **Create repository**.
2. Fill the repository name and create the repository.
3. GitHub may keep the button in a `Creating repository...` state after navigation appears complete. Wait for the repository URL before continuing.
4. If the repository already exists or contains commits, stop and switch to the safe existing-repository workflow instead of uploading a duplicate project.

## Initialize an Empty Repository

An empty repository has no default branch, so `/upload/main` cannot be used until `main` exists.

1. Prefer creating `.gitignore` first at `/new/main?filename=.gitignore`.
2. Paste the verified project ignore rules into the editor and commit them.
3. Alternatively create `README.md` first when no ignore rules are needed.
4. Verify that `main` and the first commit now exist.

## README and Source Upload

1. Create or update `README.md` at `/new/main?filename=README.md` for a new file, or edit the rendered README for an existing file.
2. Use a factual short description, features, and usage. Match the local README content; do not maintain a different web-only version.
3. Navigate to `/upload/main` to upload the remaining tracked source files.
4. Use a file chooser rather than manually transcribing large files. In the Codex browser runtime, the pattern is:

```js
const chooserPromise = tab.playwright.waitForEvent("filechooser");
await tab.click(uploadButtonElementIndex);
const chooser = await chooserPromise;
await chooser.setFiles([
  "C:\\absolute\\path\\index.html",
  "C:\\absolute\\path\\styles.css",
  "C:\\absolute\\path\\app.js"
]);
```

5. Wait until all filenames finish uploading, remove any unintended file, set a meaningful commit summary, and commit directly to the intended branch unless the user requested a pull request.
6. Verify that every expected file appears in the repository tree.

## Create a Release

1. Open `/releases/new`.
2. Select or create the exact authorized tag, such as `v1.0.0`, targeting the verified branch.
3. Enter the release title and notes in the Write tab.
4. Leave the release as production-ready unless the user requested a pre-release.
5. Attach binaries only when explicitly requested. Otherwise publish with the automatically generated **Source code (zip)** and **Source code (tar.gz)** archives.
6. Click **Publish release**, wait for `/releases/tag/<tag>`, and verify the tag, target commit, notes, and assets.

## Browser Runtime Notes

- Re-read the accessibility tree after navigation or asynchronous uploads; do not reuse stale element indexes.
- Mark user-facing or cross-turn tabs as deliverable so they remain available.
- Keep browser interactions semantic where possible. Use coordinate fallback only when the accessibility API cannot operate the control.
- If the browser session is lost, reopen GitHub and ask the user to authenticate again rather than selecting an arbitrary browser profile.
