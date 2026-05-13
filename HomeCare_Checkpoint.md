# 🏠 Home Care App — Master Checkpoint
*Last updated: May 9, 2026*

---

## 🌐 LIVE APP

| Item | Value |
|------|-------|
| **Live URL** | https://gorgeous-frangollo-7cbddc.netlify.app |
| **Hosting** | Netlify (free, permanent) |
| **Database** | Firebase Realtime Database (free Spark plan) |
| **Firebase project** | homecare-6854d |
| **Admin PIN (default)** | 1234 — changeable in Settings |
| **Venus PIN (default)** | 5678 — changeable in Settings |
| **App version constant** | `v3-may4-fresh` |

---

## 👥 PEOPLE & ROLES

- **Admins:** Eran & Yahalom (husband & wife) — full access
- **Cleaner:** Venus — restricted view-only on payments + Settings tab blocked

---

## 📅 SCHEDULE RULES

- Cleaning days: **Monday and Thursday only**
- Standard hours: **7 per visit**
- Schedule **start date: April / Monday May 4, 2026** — no visits generated before this
- Auto-generates visits for the current calendar year (Mondays & Thursdays from May 4 onwards)
- Past dates (before today) default to "Worked"
- Today + future dates default to "Upcoming"
- Admin can add extra one-off visits via Settings → Add Extra Visit
- Ontario statutory holidays auto-flagged with ★ (visits still generate; admin can cancel manually if needed)

---

## 💰 PAYMENT DEFAULTS

| Item | Value |
|------|-------|
| Hourly rate | $25.00 / hour |
| Commute reimbursement | $8.00 / visit |
| Standard visit (7h) | $183.00 |
| Currency | Canadian dollars (`$X.XX`) |
| Date locale | `en-CA` |
| Payment method | E-transfer |
| Payment frequency | Biweekly (every 2 weeks) |

---

## 🚦 VISIT STATUSES

| Status | Emoji | Color | Notes |
|---|---|---|---|
| Upcoming | ⏳ | Blue (#3b82f6) | Default for future visits |
| Worked | ✅ | Teal (#0d9488) | Default for past visits |
| Paid | 💰 | Green (#16a34a) | Locked once recorded — paidAmount immutable |
| Cancelled | ❌ | Red (#dc2626) | |
| Rescheduled | 🔄 | Amber (#d97706) | |

Admin can change any non-paid visit's status freely in any direction. Once a visit is paid, no buttons are shown to revert it (deliberate — protects payment audit trail).

---

## 🔐 ACCESS CONTROL — WHAT VENUS CAN VS CANNOT DO

### Venus CAN
- View calendar / schedule
- View payment tab (unpaid balance, rate summary, recent payments)
- View any visit's details (status, paid record, receipt thumbnail)
- Add / edit / delete **her own** chat messages (Day Chat + global Messages)
- Attach photos to her chat messages
- Generate and print payment summary reports
- Toggle language (English ↔ Filipino)

### Venus CANNOT
- Open Settings tab (removed from bottom nav + lock screen if reached)
- Change hourly rate, commute, or any PIN
- Add extra visits
- Mark any visit paid / change status / change hours
- Set or upload payment receipts
- See the "Hours worked / amount calculation" gray box on visit details (admin-only)
- Edit or delete admin's messages (only her own)

### Visual feedback for Venus
- "👁 View only — payments managed by Eran & Yahalom" pill banner at top of Payment tab
- Settings tab shows "🔒 Admin access only" with subtitle if somehow reached

---

## 🎯 FEATURE SUMMARY BY TAB

### Login screen
- Two buttons (no role labels): 🔑 **Eran & Yahalom** and ✨ **Venus**
- PIN entry on second screen (4-digit-style large input)
- Wrong PIN error message
- Language toggle in top-right

### 📅 Schedule / Calendar tab
- Monthly grid (Sun–Sat)
- Color-coded status emojis on each visit day
- Ontario stat holidays flagged with ★
- 💬 indicator on days that have any chat messages
- Tap any visit → bottom-sheet visit detail modal
- Prev/next month navigation
- Legend at bottom

### Visit Detail Modal (bottom sheet)
- Date, day of week, holiday name, "extra visit" badge
- Color-coded status badge
- **For admin only:** gray "Hours worked" box with editable input + live amount calculation
- **Payment Record card** (when paid): date paid, amount transferred, receipt thumbnail
- **Status buttons** (admin only, hidden when paid): Upcoming / Worked / Cancelled / Rescheduled
- **Record Payment form** (admin only, when status = Worked):
  - Payment date picker (defaults to today, fully editable)
  - Custom amount field (optional, blank = auto-calculated)
  - Receipt / proof of transfer upload (optional, image)
  - Confirm Payment button
- **Day Chat** — per-visit chat thread, both admin & Venus can post
  - Text + photo upload (📷 button)
  - Edit/Delete buttons under your own messages
  - "(edited)" marker after edits
  - Two-tap confirm pattern for delete

### 💰 Payment tab
- Payment reminder banner (admin only, when 14+ days since last payment OR first worked visit)
- "👁 View only" pill (cleaner role only)
- Unpaid balance card (teal gradient, big number)
- Rate summary card showing **today's effective rate** (Hourly / Commute / Per Visit)
- Awaiting Payment list (one-tap "Mark Paid" → opens visit modal for date picker flow)
- Recent payments list (last 5)
- Generate Report section: From/To date pickers + button
- Report view: full table with grand total, Print/Save as PDF button

### 💬 Messages tab (global chat)
- Shared chat between admin and Venus
- Text + photo upload
- Edit/Delete on own messages with "(edited)" marker
- Admin's messages right (teal), Venus's left (white)
- Tap any image to enlarge full-screen
- Red unread badge on tab when Venus posts (clears when admin opens tab)

### ⚙️ Settings tab (admin only)
- **Pay Rates section:** Hourly + Commute inputs, live preview of standard visit cost
  - When rates change, a yellow "📅 New rates effective from" date picker appears (defaults to today)
- **📜 Rate History card:** read-only list of every rate change with effective date, rates, and per-visit total
- **Access PINs section:** Admin PIN + Venus PIN
- **Add Extra Visit section:** date picker → adds one-off visit
- Save Changes button (active only when something modified)
- When rates change, save creates a new rate history entry

### Universal features
- Sticky header (teal gradient): app icon, role badge, language toggle, sign-out
- Bottom nav: 3 tabs for Venus, 4 for admin
- 🇨🇦 English / 🇵🇭 Filipino toggle (full bilingual UI)
- **15-minute idle auto-lock:** any tap, key press, or scroll resets the timer; signs out automatically on idle
- Real-time sync via Firebase polling every 7 seconds
- Generic image-zoom overlay (tap any photo / receipt to enlarge)
- Print styles hide nav and chrome

---

## 🗂️ DATA STRUCTURES

### Visit
```json
{
  "id": "2026-05-04",
  "date": "2026-05-04",
  "status": "worked",
  "hours": 7,
  "paidDate": null,
  "paidAmount": null,
  "receipt": null,
  "note": "",
  "comments": [],
  "isExtra": false
}
```

### Comment (in Day Chat)
```json
{
  "id": 1715257800000,
  "sender": "admin",
  "name": "Eran / Yahalom",
  "text": "Please focus on the kitchen today",
  "time": "May 9, 02:30 PM",
  "image": "data:image/jpeg;base64,..." ,
  "edited": true
}
```

### Message (global chat)
Same shape as Comment.

### Settings
```json
{
  "hourlyRate": 25,
  "commuteRate": 8,
  "adminPIN": "1234",
  "cleanerPIN": "5678",
  "rateHistory": [
    {
      "effectiveDate": "2026-05-04",
      "hourlyRate": 25,
      "commuteRate": 8,
      "changedAt": "2026-05-04T00:00:00.000Z",
      "note": "Initial rate"
    }
  ]
}
```

### Rate History rules
- Initial entry seeded on first load using the current rate values
- Each rate change appends a new entry with admin-specified `effectiveDate`
- `getRate(date)` finds the most recent entry where `effectiveDate <= date`
- Paid visits use their stored `paidAmount` (immutable); unpaid use date-aware lookup
- Rate summary on Payment tab shows `getRate(TODAY)` so future-dated changes don't mislead

---

## 🔧 TECH STACK

- **Single file:** `HomeCare.html` — no build step, no server
- **React 18** via unpkg CDN
- **Babel standalone** for JSX in browser
- **Firebase Realtime Database** for cross-device sync
- **localStorage fallback** if Firebase config not provided
- **Storage keys:** `hc/visits`, `hc/msgs`, `hc/sett`, `hc/unread`, `hc/ver`
- **Image compression:** client-side via canvas, JPEG quality 0.7 (receipts) / 0.65 (chat photos), max 720–800px wide
- **Font:** Nunito via Google Fonts
- **Design tokens:** primary `#1a6b72`, accent `#e8a020`, background `#f8f5f0`
- **Mobile-first:** max-width 480px centered

### Firebase config (already embedded)
```
API Key:      AIzaSyCNQzenUAobvTg59CFhVfmFXIqlgobKL5M
Database URL: https://homecare-6854d-default-rtdb.firebaseio.com
Database rules: { ".read": true, ".write": true }
```

### App version flow
- `APP_VERSION = 'v3-may4-fresh'` is the schema version
- On load, if `hc:ver` doesn't match → wipes visits, messages, unread (keeps settings)
- This is how the May 4 reset was triggered originally
- **Don't bump the version unless you intentionally want to wipe visit data**

---

## 🌐 LANGUAGE

Full bilingual: **English / Filipino (Tagalog)**

Key translations:
Schedule/Iskedyul · Payment/Bayad · Messages/Mensahe · Settings/Setting · Upcoming/Paparating · Worked/Nagtrabaho · Paid/Nabayaran · Cancelled/Nakansela · Rescheduled/Muling Iskedyul · Holiday/Pista Opisyal · Sign In/Mag-sign In · Sign Out/Lumabas · Edit/I-edit · Delete/Burahin · Save/I-save · Cancel/Kanselahin · edited/na-edit · View only/Tingnan lamang

---

## ✅ DECISIONS ALREADY MADE

- Admin icon is 🔑 not 👑
- Login buttons show names only — no "Admin" / "Cleaner" subtitle
- Schedule starts **May 4, 2026** — nothing before that
- Past visits before today default to "Worked"
- All 5 statuses freely switchable for non-paid visits; paid visits have no revert button
- Venus sees Payment tab balance + history but cannot mark paid or change anything
- Ontario stat holidays only
- Currency: Canadian dollars `$X.XX`
- Dates: `en-CA` locale
- Auto-lock after **15 minutes of inactivity** (any role)
- Payment reminder triggers after **14 days** since last payment OR first worked visit (whichever applies)
- Venus does NOT see the gray "Hours worked / amount calculation" box on visit details
- Rate changes log to `rateHistory`; effective date selectable; past visits keep their old rates
- Image uploads compressed client-side to keep Firebase storage manageable
- Edit/Delete only on own messages; deleted messages disappear (no soft-delete)
- Two-tap confirm pattern for delete (no browser confirm dialogs)
- Single cleaner only (no plans for multi-cleaner support)

---

## 🐛 KNOWN BEHAVIOURS / GOTCHAS

- Components inside App must be invoked as **`{TabName()}`** not `<TabName />`. Calling them as React components causes the input to remount on every keystroke and lose focus. (Bug fixed earlier today.)
- Firebase rules are wide open (`.read: true, .write: true`). Acceptable for a household app between trusted parties; not suitable for production multi-tenant use.
- Image data is stored as base64 directly in Firebase Realtime DB. Free tier is 1GB total. Compressed images are ~50–150KB each, so capacity is plenty for personal use but won't scale to thousands of users.
- Auto-lock checks every 20 seconds — actual sign-out can lag up to 20s past the 15-min mark.
- iOS standalone PWA (added to home screen) works well, but the keyboard-on-input bug fix is critical for the chat to be usable.
- "(edited)" tag is appended to timestamp; once edited, a message cannot be marked as un-edited.

---

## 🔄 HOW TO UPDATE THE APP IN THE FUTURE

1. Open a new Claude conversation
2. Upload **this checkpoint file** + the current **`HomeCare.html`**
3. Say: *"I have the Home Care app checkpoint. I want to [describe change]."*
4. Claude edits the app and gives you a new `HomeCare.html`
5. Validate Babel JSX compiles (Claude does this automatically)
6. Go to **app.netlify.com** → your project → drag the new file onto "Production deploys"
7. Live URL stays the same — no need to re-share with Venus or Yahalom
8. **Update this checkpoint** with whatever changed, replace it in your project folder

---

## 📝 RECENT CHANGE LOG (May 9, 2026 session)

| # | Change |
|---|---|
| 1 | Added **Day Chat** (per-visit chat thread) with text messages |
| 2 | Made **Payment Record card** prominent (date paid + amount transferred) |
| 3 | **Wiped all history**, schedule starts fresh from May 4, 2026 |
| 4 | **Date picker for payment** — admin specifies when payment was made (default: today) |
| 5 | **Receipt upload** — attach e-transfer screenshot when marking paid (compressed client-side) |
| 6 | **Click-to-zoom** image viewer (works for any image: receipts, chat photos) |
| 7 | Fixed **mobile keyboard losing focus on every keystroke** in chat (component-as-function fix) |
| 8 | Fixed **From/To date picker alignment** in Generate Report (grid + explicit height) |
| 9 | Tightened **Venus's view-only access** + visible "View only" pill on Payment tab |
| 10 | Hidden **gray "Hours worked" box** from Venus on visit details |
| 11 | **Rate history** with effective dates — new rates apply only to future visits |
| 12 | **Photo upload** in both Day Chat and global Messages |
| 13 | **Biweekly payment reminder banner** (14+ days from last payment or first worked visit) |
| 14 | **15-minute PIN auto-lock** on idle |
| 15 | **Edit / Delete** own messages with "(edited)" marker and two-tap delete confirm |

---

*Built with Claude · Hamilton, Ontario, Canada*
