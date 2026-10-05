# BIS Review Dashboard — Login + Upload System

Is project ka goal: aapka existing HTML dashboard ab sirf ek static file nahi rahega —
isme login (Gmail se), aur "time to time report upload karna" ka system jud jayega.
Sab kuch **free** rahega — GitHub (code store) + Firebase (login/database/storage).

---

## Poora Roadmap (hum step-by-step karenge)

- [x] **Step 1 — Login Panel** — Google Sign-In working page
- [x] **Step 2 — Firebase Project Setup** — project `bis-dashboard-709e4` live hai
- [x] **Step 2.5 — GitHub Pages par deploy** — live link: https://rituraj11-code.github.io/BIS-Dashboard/
- [x] **Step 3 (Phase 1) — Upload Panel** — abhi sirf "Cards Generated" form (`upload.html`)
- [ ] **Step 3 (Phase 2)** — Eligible / Pending / Period report ke forms isi panel mein jodna
- [ ] **Step 4 — Dashboard ko Firebase se jodna** — taaki dashboard Firestore se latest data
      khud utha le, hardcoded numbers ki jagah
- [ ] **Step 5 — Security Rules** — abhi Firestore "test mode" mein hai (koi bhi likh sakta hai agar
      link mil jaye) — baad mein isko "sirf logged-in users" tak restrict karenge

Har step ek chhota, kaam karne wala piece hoga — pehle poora system ek saath nahi banayenge.

---

## Step 1 — Login Panel (abhi ka kaam)

File: `login.html`

Ye kya karta hai:
- "Sign in with Google" button — koi bhi apne Gmail se login kar sakta hai
- Login ke baad user ka naam/photo dikhata hai, aur ek "Logout" button
- Abhi ye sirf **login dikhane** tak kaam karta hai — ye kisi real Firebase project se
  judega jab aap Step 2 (Firebase setup) kar loge

### Ye file abhi kaam kyun nahi karegi (jab tak Step 2 na ho)

`login.html` ke andar ek jagah hai:

```js
const firebaseConfig = {
  apiKey: "YOUR_API_KEY",
  authDomain: "YOUR_PROJECT.firebaseapp.com",
  projectId: "YOUR_PROJECT_ID",
  storageBucket: "YOUR_PROJECT.appspot.com",
  messagingSenderId: "...",
  appId: "..."
};
```

Ye placeholder hai. Jab aap Firebase par apna free project banaoge, Firebase aapko
yahi cheez (apni keys ke saath) de dega — bas copy-paste karna hoga, code badalna nahi padega.

### Step 2 ke liye jab ready ho, ye karna (10 min):

1. https://console.firebase.google.com par jao, Gmail se login karo
2. "Add Project" → naam do jaise `bis-dashboard` → create
3. Left menu me **Build → Authentication** → "Get Started" → **Google** provider ON karo
4. Left menu me **Build → Firestore Database** → "Create Database" → test mode me start karo
5. Project Settings (gear icon) → "Your apps" → **Web app (</>)** add karo →
   Firebase wahan aapko `firebaseConfig` object dega — wahi copy karke mujhe bhej dena
   (ya khud `login.html` me paste kar dena), main agla step uske upar banaunga

---

## Step 3 (Phase 1) — Upload Panel

File: `upload.html`

Kya karta hai:
- Login zaroori hai — agar login nahi kiya hoga to seedha "Login page par jao" dikhega
- Report type chuno (abhi sirf **"Cards Generated (District-wise)"**) aur date chuno
- Saare 23 districts ki table dikhegi — SECC, Tagged, VVS, CHIRAYU, CHIRAYU Ext., CCHF Employee,
  CCHF Pensioners, ASHA, Anganwadi Helper, Anganwadi Worker, Divyang — har column mein number daalo
- Total khud ban jata hai (row-wise aur neeche grand total)
- **"Save Report"** dabane par data Firestore database mein chala jata hai
  (collection: `reports`, document id jaisे `cards_generated_2026-10-05`)

### Test karne ka tarika
1. `upload.html` ko GitHub pe upload karo (login.html/index.html ke saath hi)
2. Live link kholo, login karo
3. "Upload Panel par jao" button dabao (ab login panel pe ye button dikhega)
4. Kisi bhi 2-3 district ki values bhar ke "Save Report" try karo
5. Firebase Console → Firestore Database mein jaake dekho — `reports` collection mein
   naya document dikhna chahiye

### ⚠️ Abhi ke liye ek dhyan rakhne wali baat
Firestore abhi **"test mode"** mein hai — matlab agar kisi ko bhi ye link mil jaye, wo
seedha database access kar sakta hai (sirf aapke GitHub repo ya login wajah se nahi rukega).
Test mode 30 din baad apne aap band ho jata hai. Jab Step 5 (Security Rules) karenge,
tab isko "sirf jo log-in hain" tak lock kar denge. Abhi testing ke liye theek hai.

Jab ye test ho jaye aur kaam kar raha lage, bata dena — hum **Step 4** (dashboard ko
is data se jodna) ya **Step 3 Phase 2** (baaki report types ke forms) — jo pehle
karna chaho, kar sakte hain.

---

## Files is folder mein

- `login.html` / `index.html` — Google Sign-In page (dono same hain, GitHub Pages ke liye)
- `upload.html` — Report upload panel (Step 3, Phase 1)
- `README.md` — ye file
