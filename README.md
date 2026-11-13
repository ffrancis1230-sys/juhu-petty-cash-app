# Juhu Petty Cash

The public GitHub Pages site hosts only the sign-in and app interface. Firebase Authentication handles Google sign-in; a hidden form POST sends each request and Firebase ID token to the private Apps Script backend. The backend verifies the token and manager/staff permissions before accessing the owner's private Google Sheet and Drive receipt folder. No sheet or receipt is published.

## What the app supports

- Personal-Gmail Google sign-in; server-side authorization for the manager and explicitly approved staff.
- Manager-only staff approval/revocation, expense editing, and expense approval/return.
- Staff submissions with server-assigned voucher numbers and idempotent retry protection.
- Staff receipt viewing/replacement only for their own submissions; managers can review the full register.
- Private Drive receipt files, versioned replacements, and authorization before image reads.
- Automatic 1–15 / 16–month-end period labels; no period worksheets are generated.
- Filterable period views and six voucher/receipt cards per A4 portrait page.

## Deployment

1. Keep Firebase project `juhu-petty-cash` on the Spark plan. Enable **Authentication → Google**.
2. Add `https://ffrancis1230-sys.github.io` to **Authentication → Settings → Authorized domains**.
3. Keep the Apps Script web app deployed as the owner and available to anyone. Its public page grants no data access; every API operation requires a verified Firebase ID token and a server-side allowlist match.
4. In the Apps Script editor, run `configureHostedFrontend_()` once as the manager. It restricts bridge responses to the configured GitHub Pages origin and makes the backend URL show the hosted app link.
5. In GitHub, enable **Settings → Pages → Deploy from a branch**, select `main` and `/(root)`. The app URL is `https://ffrancis1230-sys.github.io/juhu-petty-cash-app/`.
6. Test the live manager, approved staff, and unapproved-account flows before entering real expenses or sharing the link with staff.

The Firebase web configuration and Apps Script web-app endpoint in `index.html` are public client settings, not secrets. Never add Firebase service-account credentials, sign-in tokens, workbook IDs, or receipt-folder IDs to this repository. Do not make the spreadsheet or Drive folder public.

## Release gate

Do not use the app for real expenses until manager/staff authorization, staff ownership boundaries, receipt replacement, voucher numbering, half-month boundaries, and A4 printing have been tested against the live deployment.
