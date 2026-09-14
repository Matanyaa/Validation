# SEM Validation — setup

This app is a single HTML file (`index.html`). No build step, no npm. It
saves its data to a Firebase Firestore document, and it's hosted for free
on GitHub Pages — same pattern as Split Builder. Two things to set up
once: Firebase (for data) and GitHub (for hosting + auto-update).

## 1. Firebase (data storage)

1. Go to [console.firebase.google.com](https://console.firebase.google.com) and sign in with your Google account.
2. **Add project** → name it e.g. `sem-validation` → you can decline Google Analytics (not needed) → Create.
3. In the left sidebar: **Build → Firestore Database → Create database**.
   - Pick any nearby location.
   - Start in **production mode** (we'll set our own rules next).
4. Go to the **Rules** tab of Firestore and replace the contents with:

   ```
   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {
       match /validation/state {
         allow read, write: if true;
       }
       match /{document=**} {
         allow read, write: if false;
       }
     }
   }
   ```

   This limits reads/writes to the single document the app actually uses
   (`validation/state`) and blocks everything else. There's no login in
   this app — anyone with the exact config *and* who knows to write to
   that exact path could write to it, same tradeoff Split Builder makes.
   Fine for a personal lab tool; don't put anything sensitive in it.
5. Click **Publish** on the rules.
6. Go to **Project settings** (gear icon, top left) → scroll to **Your apps** → click the **</>** (web) icon → nickname it `SEM Validation` → **Register app**. Don't bother with Firebase Hosting here, we're using GitHub Pages.
7. Copy the `firebaseConfig` object it shows you (looks like the block below) and paste it into `index.html`, replacing the placeholder near the bottom of the `<script>` block (search for `YOUR_API_KEY`):

   ```js
   const firebaseConfig = {
     apiKey: "...",
     authDomain: "...",
     projectId: "...",
     storageBucket: "...",
     messagingSenderId: "...",
     appId: "...",
   };
   ```

That's it — once this is pasted in and pushed (step 2 below), the app
will show "Saved ✓" / "Loaded from cloud" in the header instead of "Not
connected".

## 2. GitHub (hosting + auto-update)

1. On [github.com](https://github.com), create a new repository (e.g. `sem-validation`). **Public** is simplest — GitHub Pages on a private repo needs a paid GitHub plan. The repo only holds app code, not your data (that's in Firestore), so a public repo is low-risk; skip this if you'd rather pay for private Pages.
2. Back in this project folder, push it:

   ```
   git remote add origin https://github.com/<your-username>/sem-validation.git
   git branch -M main
   git add -A
   git commit -m "Initial SEM validation app"
   git push -u origin main
   ```

3. On GitHub: **Settings → Pages** → under "Build and deployment", set **Source** to "Deploy from a branch", **Branch** to `main` / `(root)` → **Save**.
4. Wait a minute or two, then your app is live at:
   `https://<your-username>.github.io/sem-validation/`

## 3. Install it on your phone

Open that URL on your phone in Chrome (Android) or Safari (iOS), then:
- **Android/Chrome**: menu (⋮) → "Add to Home screen" / "Install app".
- **iOS/Safari**: Share icon → "Add to Home Screen".

It'll behave like a native app icon, opening full-screen without browser
chrome.

## 4. How auto-update works

There's no app store step. When you (or I, on your behalf) push a change
to `index.html` on `main`, GitHub Pages redeploys it automatically within
a minute or two. The app itself checks the live `index.html` for a
version bump every time you open it or bring it to the foreground, and
if it finds a newer version it shows a banner with a **Reload now**
button — same mechanism as Split Builder. So: push → wait ~1 min →
reopen the app on your phone → tap Reload when the banner shows up.

## Making future changes

Any time you want a feature added or a bug fixed, just describe it and
I'll edit `index.html` directly, bump `APP_VERSION` near the top of the
script, and push — no rebuild step involved anywhere.
