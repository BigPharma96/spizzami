# SpizzaMi — setup

SpizzaMi is a web app that installs like a native app on Android and Windows.
It has no server: your papers live in a folder called **SpizzaMi** in your own
Google Drive, and every device you sign in from sees the same library.

Setup is two jobs, done once: get a key from Google, and put the app online.
Budget about twenty minutes.

---

## 1. Put the app online

A web app can only be installed if it is served over HTTPS, so the files need a
home. GitHub Pages is free and takes five minutes.

1. Create a free account at github.com if you don't have one.
2. Create a new repository, name it `spizzami`, and set it to **Public**.
   (Public means the *code* is visible, not your papers — those stay in your Drive.)
3. Upload every file from this folder: `index.html`, `config.js`, `sw.js`,
   `manifest.webmanifest`, `icon-192.png`, `icon-512.png`, `README.md`.
4. Go to **Settings → Pages**, and under "Branch" pick `main` and `/ (root)`, then Save.
5. After a minute the page shows your address. It looks like:
   `https://yourname.github.io/spizzami/`

Write that address down — Google needs it in step 2.

---

## 2. Get your Google key

1. Go to `console.cloud.google.com` and sign in.
2. Top left, click the project selector → **New project**. Name it `SpizzaMi`, create it,
   then make sure it is the selected project.
3. In the search bar type **Google Drive API**, open it, and press **Enable**.
4. Go to **APIs & Services → OAuth consent screen**.
   - User type: **External**. Create.
   - App name: `SpizzaMi`. Your email in both the support and developer contact fields.
   - Save and continue through the Scopes screen without adding anything.
   - On **Test users**, add your own Gmail address, plus any friend you want to give
     the app to. Up to 100 people. Save.
5. Go to **APIs & Services → Credentials → Create credentials → OAuth client ID**.
   - Application type: **Web application**. Name: `SpizzaMi web`.
   - Under **Authorised JavaScript origins**, click Add URI and enter the origin
     of your address — the domain only, no path:
     `https://yourname.github.io`
   - Create. Google shows you a **Client ID**. Copy it.
6. Open `config.js`, paste the Client ID between the quotes, save, and upload the
   updated `config.js` to your repository.

That's it. Open your address, press **Sign in with Google**, and accept the
permission request. Google will warn that the app is not verified — that is
expected while it is in testing mode, and you get past it with
*Advanced → Go to SpizzaMi*.

---

## 3. Install it on your devices

**Android (Chrome):** open the address, then the menu (⋮) → *Install app* or
*Add to home screen*.

**Windows (Chrome or Edge):** open the address, then the install icon at the right
edge of the address bar, or menu → *Cast, save and share → Install*.

It then behaves like an installed program: its own icon, its own window, no browser
chrome around it.

---

## How it works, in one paragraph

The app asks Google for one narrow permission, `drive.file`, which grants access
only to files the app itself created. It makes a `SpizzaMi` folder in your Drive
holding one `library.json` index plus a PDF and a cover image per paper. The index
is what the Library and List screens read, which is why they open instantly and
still work offline; PDFs are fetched from Drive only when you open one.

**One consequence worth knowing:** because of that narrow permission, dropping a PDF
into the Drive folder by hand does nothing — the app cannot see files it did not
create. Always add papers through *Add paper*.

---

## If something goes wrong

**"Setup needed: add your Google Client ID"** — `config.js` still has the placeholder,
or the updated file hasn't finished uploading to GitHub Pages.

**Sign-in popup closes immediately, or `origin_mismatch`** — the address you are
visiting is not in *Authorised JavaScript origins*. It must match exactly, scheme
included, with no trailing slash and no path.

**"Google hasn't verified this app"** — normal in testing mode. If instead it refuses
outright, the account you are using is not in the Test users list.

**Session expired** — access tokens last an hour. Press Reconnect; nothing is lost.

**Metadata comes out wrong** — expected. Journal and first author are almost never
detectable from the text layer, and the title is only as good as the PDF's own
formatting. The review screen is where the record actually gets made.

---

## Sharing it with a friend

Send them your address and add their Gmail to Test users in the consent screen.
They sign in with their own account and get their own separate library in their own
Drive. Nothing of theirs is visible to you.

To go beyond 100 people you would publish the app, which for the `drive.file`
permission means a short brand-verification review by Google, not the expensive
security audit that the broader Drive permissions require.
