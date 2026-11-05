# BLRS — Step-by-Step Deployment Guide

You will upload **3 files** to GitHub, paste **security rules** into Firebase, then refresh every device.
Takes about 15 minutes. No coding needed.

| File | What it is |
|---|---|
| `index.html` | The whole app (new photo, new header, security fixes) |
| `vercel.json` | Security settings for the website (blocks fake copies of your site inside other pages) |
| `firestore.rules` | Security rules for the database (you copy-paste its text into Firebase — Part B) |

---

## Part A — Upload the files to GitHub (website update)

1. **Make a backup first.** Open BLRS → log in as **Livestock Focal Officer** → **💾 Backup & Sync** → **⬇️ Download Full Backup**.
2. Download the 3 files from the Claude chat (click each card → **Download**). They go to your **Downloads** folder.
3. Go to **github.com/pvsoaew-design/livestock-registry** (make sure it says branch **main**).
4. Click **Add file → Upload files**.
5. Drag **index.html**, **vercel.json** and **firestore.rules** onto the page (all three together).
6. Scroll down → click the green **Commit changes** button.
7. Wait **1–2 minutes**. Vercel publishes the new version automatically.
   (Optional check: vercel.com → project **livestock-registry** → **Deployments** → newest one says **Ready**.)

## Part B — Lock the database (Firebase rules) — very important

1. Go to **console.firebase.google.com** and sign in with the Google account that owns the project.
2. Click the project **hinunangan-livestock-registry**.
3. Left menu: **Build → Firestore Database** → click the **Rules** tab at the top.
4. Open **firestore.rules** on your computer with Notepad (right-click → Open with → Notepad), press **Ctrl+A**, then **Ctrl+C**.
5. In Firebase, click inside the rules box, press **Ctrl+A** (select all) and **Ctrl+V** (paste) to replace the old rules.
6. Click **Publish**.

What these rules do: only the app can read/write; nobody can delete the registry; a single save cannot wipe out more than 50 farmers; everything else in the database is closed.

## Part C — Restrict the Firebase key to your website (recommended, 5 minutes)

1. Go to **console.cloud.google.com** → choose project **hinunangan-livestock-registry** (top bar).
2. Menu ☰ → **APIs & Services → Credentials**.
3. Under **API Keys**, click the key named like **Browser key (auto created by Firebase)**.
4. Under **Application restrictions** choose **Websites**, then **Add** these two lines:
   - your website address, e.g. `https://livestock-registry.vercel.app/*` (use your real address)
   - `https://hinunangan-livestock-registry.firebaseapp.com/*`
5. Click **Save**. (Takes up to 5 minutes to apply.)

## Part D — Refresh every device (important)

- **Computer:** open BLRS and press **Ctrl + F5**.
- **Phone:** close the browser tab completely and open BLRS again.

Do this on **every** device, so no one keeps using the old version.

## Part E — Check that it works

1. Log in as a **Barangay Secretary** with your usual PIN → it still works (old PINs are kept, now stored scrambled).
2. Log in as **Focal** → if you see a reminder about a security question, go to **🔑 Account Management → Change My Access Code** and set one.
3. **💾 Backup & Sync** → you should see **🗓️ Automatic Daily Snapshots**.
4. Bottom-right badge should say **☁️ Synced**.

## If something goes wrong

- **Undo the website update:** vercel.com → **livestock-registry** → **Deployments** → the previous one → **⋯ → Promote to Production**. Your data is in Firebase, so nothing is lost.
- **"Not synced" right after Part B:** check the rules were pasted completely and **Published**.
- **Focal forgot the access code (no security question set):** Firebase console → **Firestore Database → Data** → `registry` → `main` → hover the field **focalAccount** → click the 🗑️ delete icon. The next Focal login will ask to create a new code. No registry data is lost.
- **Need to restore an old backup with far fewer farmers:** in the Firebase rules change `- 50` to `- 5000`, Publish, do the restore, then change it back and Publish.

## What changed in this version (security)

- PINs, the Focal access code and the security answer are stored **scrambled (hashed)** — they can no longer be read from the database or shown on screen.
- **5 wrong PINs = temporary lock** (30 seconds, growing up to 15 minutes).
- **Auto log-out after 15 minutes** of no activity (warning 1 minute before).
- If a PIN is **reset or changed**, devices still logged in with the old one are **logged out**.
- Easy PINs like 1111 / 1234 are refused for **new** accounts; new Focal codes need 6+ characters.
- The old "type RESET" option that let anyone reset the Focal code is **removed**.
- Data from the cloud or a backup file is **checked and cleaned** so it cannot inject harmful code.
- The page **cannot be embedded** in another website (prevents look-alike / click-trick attacks), and only trusted script sources are allowed.
- **Automatic daily snapshots** (last 7 days) on each device, with one-click **Merge** to recover lost records.
- Secretaries/agents cannot remove more than 5 farmer records in one save; the database rules block mass deletions too.
