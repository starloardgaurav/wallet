# Document Wallet

A personal, password-protected web page for keeping important documents (certificates, marksheets, ID proofs and similar) in your own Google Drive, encrypted before they leave your browser.

This repository contains only the page's code. **It does not contain anyone's documents, passwords or personal data.**

## Features

- Wallet-style cards with category artwork. Click a card to flip it and preview the file.
- Light glass design with a dark mode switch.
- Search, categories and tags.
- Expiry dates with "days left" badges and a warning banner.
- Merge several PDFs and images (JPG, PNG) into one PDF.
- Auto-lock after 5 minutes of inactivity.
- Works on any device with a browser.

## How it works

1. You enter a password in your browser.
2. The password is turned into an encryption key (PBKDF2, SHA-256, 310,000 rounds).
3. Each file and its details (name, title, tags, expiry) are encrypted with AES-256-GCM.
4. Only the encrypted data is uploaded, to a hidden app folder in **your own** Google Drive (the `drive.appdata` scope). The page cannot see any other file in your Drive.
5. To open a document, the page downloads the encrypted copy and decrypts it in your browser.

The password is never stored or sent anywhere. Google, GitHub and the page's author cannot read your documents.

## Privacy and security notes

- Every user's data stays in their own Google account. Nothing is shared between users.
- The Google OAuth Client ID is public by design and is not a secret. It identifies the app and gives no access to anyone's data.
- **If you forget your password, your documents cannot be recovered.** Keep your original files somewhere safe too.
- Use a long, unique password and turn on 2-step verification for your Google account.
- Do not use it on shared or public computers. Press Lock when you finish.
- This is a personal project. It has not had a professional security audit. Use it at your own risk.

## Set up your own copy

You need a free Google account and a free GitHub account.

### 1. Get a Google Client ID

1. Go to [Google Cloud Console](https://console.cloud.google.com) and create a new project.
2. Open **APIs & Services → Library**, find **Google Drive API** and click **Enable**.
3. Open **APIs & Services → OAuth consent screen**, choose **External** and fill in an app name and your email.
4. Add the scope `https://www.googleapis.com/auth/drive.appdata`.
5. Under **Audience → Test users**, add your own Gmail address. Keep the app in **Testing** mode so only listed accounts can sign in.
6. Open **Credentials → Create credentials → OAuth client ID**, choose **Web application**.
7. Under **Authorised JavaScript origins**, add your site address, for example `https://yourusername.github.io` (no path and no trailing slash).
8. Copy the **Client ID** (it ends in `.apps.googleusercontent.com`).

### 2. Publish with GitHub Pages

1. Fork or copy this repository to your own account.
2. Open **Settings → Pages**, choose the `main` branch as the source and save.
3. Your site will appear at `https://yourusername.github.io/your-repository/`.

### 3. First use

1. Open your site and paste your Client ID into the box that appears (once per browser).
2. Enter a password (at least 8 characters) and press **Unlock**.
3. Sign in with Google in the popup and allow access. If you see "app isn't verified", choose **Continue**. This is normal for a personal app in Testing mode.
4. The page tells you no vault exists. Confirm your password to create it.

From then on, unlocking needs only your password (plus a Google sign-in when the session expires).

## Troubleshooting

| Problem | Likely cause |
| --- | --- |
| Stuck on "Waiting for Google sign-in" | The popup is blocked. Allow popups for the site. |
| `origin_mismatch` | The authorised origin in Google Cloud does not exactly match your site address. |
| `access_denied` | Your Gmail is not listed under Test users. |
| 403 error | The Google Drive API is not enabled for your project. |
| Console warnings about Cross-Origin-Opener-Policy | Harmless messages from Google's sign-in popup. |

## Built with

- Plain HTML, CSS and JavaScript in a single file (`index.html`)
- Web Crypto API for encryption
- [Google Identity Services](https://developers.google.com/identity/oauth2/web/guides/overview) and the Google Drive API
- [pdf-lib](https://pdf-lib.js.org) for merging PDFs

## Disclaimer

Provided as is, without warranty of any kind. The author is not responsible for lost data or forgotten passwords.
