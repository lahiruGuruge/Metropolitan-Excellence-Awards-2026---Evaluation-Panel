# Metropolitan Excellence Awards 2026 — Recipient Selection

Browser-based tool for reviewing award recipients, updating selections, saving the current set in the browser, and printing an A4 sign-off sheet for the evaluation panel.

## Use

1. Open `index.html` in a modern browser (or host it as a static page).
2. Select **Load Award Selection workbook** and choose the current `.xlsx` workbook. The app reads the `Award Selection` worksheet and imports its categories, entry counts, and recipient values.
3. Edit any recipient field. Blank values remain marked **Pending**.
4. Choose **Save selections** to save in the browser profile on that device. Search and status filters help review the list.
5. Select the eight panel photos in the order shown on the page, then choose **Print / Save as PDF**. Printing uses A4 portrait: the award list is on page one and the signature blocks are on page two.

The app does not upload the workbook or photos. Recipient changes are stored in browser `localStorage`; the panel photos are held as temporary local object URLs for the current page session. Save a fresh copy of the workbook or print/PDF if you need a portable record.

## Authentication and request flow

### Application authentication

The repository was empty before this page was added, so it had no pre-existing login, session, authentication middleware, backend, or token handling. The page itself still has **no application login**. It is a static client-side page and does not make an authenticated request to a server. Anyone who can open the hosted page can see its interface. Selection data is loaded from a file chosen in the browser and saved only to that browser's local storage.

The page loads the SheetJS library from jsDelivr to parse Excel files. The workbook is read with the browser File API (`file.arrayBuffer()` and `XLSX.read`) and is not sent with that library request. Printing uses the browser print dialog. Panel photos are selected locally and rendered with `URL.createObjectURL()`; they are not uploaded.

### GitHub repository authentication

GitHub repository access is separate from application authentication. This repository is currently public, so anyone can read its committed files. Writing a commit requires a GitHub identity or authorized integration with write permission. In this session, repository reads and writes were made through the connected GitHub integration, which handled its authorization outside this page. No GitHub password, access token, or other repository credential is present in the app source. The page does not call GitHub APIs and cannot read the integration's credentials.

The repository connector's write flow for this change was: identify the repository and default branch → submit a UTF-8 file to GitHub's Contents API → GitHub creates the content and commit. GitHub's API requires appropriate repository contents permission for writes; the authentication credential is supplied by the authorized client, never by this page. See GitHub's documentation on [REST API authentication](https://docs.github.com/en/rest/authentication) and [repository contents](https://docs.github.com/en/rest/repos/contents).

### Data and deployment boundary

The source code is in a public repository, but recipient selections and photos are not committed. A static page has no access control, so do not add award decisions to this repository. Browser local storage is device- and browser-profile-specific; it is not a shared database, and clearing browser data removes the saved copy. If the page needs to be hosted for a limited panel, put it behind an authenticated service and store selections in a protected backend before distributing its URL.
