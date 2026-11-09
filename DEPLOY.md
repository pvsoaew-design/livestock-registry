# BLRS — Update & Rollout Guide (version 2026.11.09: "one storage space per barangay")

This update fixes **"Cloud copy too large — clear old notifications/sales"**, records not saving, and the
notification "jam" when many devices are used at once. It gives **each barangay its own cloud storage space**
and moves the registry on each device into the browser's larger database.

**Do the steps in this exact order.** About 30 minutes. No coding needed.

| File | What it is |
|---|---|
| `index.html` | The app |
| `firestore.rules` | New database rules (you copy-paste its text into Firebase — Step 2) |
| `vercel.json` | Website security settings (same as before) |

---

## Step 1 — Back up (2 minutes)

1. Open BLRS → log in as **Livestock Focal Officer** → **💾 Backup & Sync** → **⬇️ Download Full Backup**.
2. Keep the file somewhere safe (e.g. Google Drive). This is your safety net.

## Step 2 — Put in the new database rules FIRST (5 minutes)

The new app saves each barangay separately; the old rules would refuse that, so the rules go in first.
(The new rules still accept the old app, so nothing breaks while you do Step 3.)

1. **console.firebase.google.com** → project **hinunangan-livestock-registry**.
2. Left menu **Build → Firestore Database** → **Rules** tab.
3. Open `firestore.rules` with Notepad → **Ctrl+A**, **Ctrl+C** → in Firebase click in the rules box → **Ctrl+A**, **Ctrl+V** → **Publish**.

## Step 3 — Upload the new app (5 minutes)

1. **github.com/pvsoaew-design/livestock-registry** (branch **main**) → **Add file → Upload files**.
2. Drag in **index.html**, **firestore.rules** and **vercel.json** → **Commit changes**.
3. Wait 1–2 minutes (vercel.com → **livestock-registry** → **Deployments** → newest says **Ready**).

## Step 4 — The Focal Officer opens it first (5 minutes)

1. On **your** computer: open BLRS → press **Ctrl + F5** → log in as **Livestock Focal Officer**.
2. Leave it open for **1–2 minutes**. In the background the app moves the registry into one storage space
   per barangay. The badge at the bottom-right shows **Saving…** then **☁️ Synced**.
3. Check: **💾 Backup & Sync → 📦 Storage** lists the barangays, each far below 100%.

## Step 5 — Refresh EVERY device (the important part)

Every computer and phone must load the new version — old open tabs can no longer save.

- **Computers:** open BLRS and press **Ctrl + F5**.
- **Phones:** close the BLRS tab completely, then open the link again.

Tell all 40 Barangay Secretaries, the 20 staff and the Mayor's office (a text message works):
> "BLRS was updated. Please close the BLRS tab and open it again. If the bottom-right badge says
> 'Not synced', close and reopen once more."

A device that was **not** refreshed shows **☁️ Not synced** and its saves are refused (this protects the data).
Nothing typed on it is lost: as soon as it is refreshed, its unsaved work uploads automatically.
Future updates will show a **🔄 new version available** message by themselves.

## Step 6 — Check (2 minutes)

- A Barangay Secretary adds a test farmer → on the Focal computer the 🔔 bell shows it within seconds.
- Bottom-right badge says **☁️ Synced** on both.

---

## Keeping storage healthy (once a month, 2 minutes)

**💾 Backup & Sync → 📦 Storage** shows how full each barangay's space is (each barangay has **1 MB**,
about **2,000 farmer/animal/sales records**). Green = fine, yellow = getting full, red = act now.

- Old notifications are removed **automatically** (read ones after 45 days; at most 120 per barangay).
- If a barangay turns yellow/red: **⬇️ Download Full Backup**, then **🧹 Clean up storage**
  (remove read/old notifications, old chat messages, or sales history before a chosen year).

## Firebase usage (once a month)

Firebase console → **Firestore Database → Usage**. The free plan allows 50,000 reads and 20,000 writes per
day and 10 GB of downloads per month. With this version, 60 devices doing a normal day's work use only a small
part of that (tested: about 30× less downloading than the previous version). Devices that are logged out —
including after 15 minutes of no activity — download almost nothing.
If you ever approach the limit, Firebase **Upgrade → Blaze** (pay-as-you-go, set a budget alert of e.g. ₱100)
prevents the app from stopping; at this size the cost is normally ₱0–₱100 per month.

## If something goes wrong

- **Undo the website update:** vercel.com → **livestock-registry** → **Deployments** → previous → **⋯ → Promote to Production**.
  Then in Firebase Rules, paste back the previous rules. Your data is safe in Firebase.
- **A device keeps saying "Not synced"** → it is on an old version: close the tab fully and reopen (phones), Ctrl+F5 (computers).
- **"Brgy. X: storage full"** → Focal: Download Full Backup → 📦 Storage → 🧹 Clean up storage.
- **"Not allowed by the database rules"** → Step 2 was not published; repeat it.
- **Focal forgot the access code (no security question set):** Firebase → Firestore → **Data** → `registry` → `main` → delete the field **focalAccount**.
- **Restoring an old backup that has many fewer farmers in a barangay** → in the rules change `- 50` to `- 5000`, Publish, restore, then change it back and Publish.
