# BIS Review Dashboard — Login + Upload System

Is project ka goal: aapka existing HTML dashboard ab sirf ek static file nahi rahega —
isme login (Gmail se), aur "time to time report upload karna" ka system jud jayega.
Sab kuch **free** rahega — GitHub (code store) + Firebase (login/database/storage).

---

## Poora Roadmap (hum step-by-step karenge)

- [x] **Step 1 — Login Panel** — Google Sign-In working page
- [x] **Step 2 — Firebase Project Setup** — project `bis-dashboard-709e4` live hai
- [x] **Step 2.5 — GitHub Pages par deploy** — live link: https://rituraj11-code.github.io/BIS-Dashboard/
- [x] **Step 3 — Upload Panel, 4 report types** — Cards Generated, Eligible, Pending, Period
      (`upload.html`) — **admin-only**, ek hi Gmail account use kar sakta hai
- [x] **Step 4 — Public Live Dashboard (view-reports.html)** — bina login ke khulta hai,
      koi bhi dekh sakta hai; Firestore se seedha data khींचकर KPI cards + scheme snapshot +
      district table dikhata hai
- [x] **Step 5 — Security Rules updated** — read sabke liye open, write sirf admin ke liye
      (neeche "Step 5" section dekho — ye rules Firebase Console mein daalni hongi)

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

---

## Step 4 — Live Dashboard (view-reports.html)

File: `view-reports.html`

Kya karta hai:
- Login zaroori hai (jaise upload panel mein)
- Upar "Report Date" dropdown — Firestore mein jitni bhi dates ki reports save hain, sab yahan
  dikhengi; koi bhi select karke dekho
- **KPI cards** — Total Cards Generated, kitne districts ki report aayi, Top Scheme
- **Scheme-wise Snapshot** — bar-chart jaisa view, har scheme ka total
- **District-wise table** — search box ke saath, sab 23 districts, sab columns, Total row bhi

Ye poora data **live** Firestore se aata hai — matlab jab bhi Upload Panel se nayi report save
karoge, yahan turant (page refresh karne par) naya data dikhega. Koi HTML edit karne ki
zaroorat nahi.

Teeno pages ab aapas mein linked hain:
- Login page → "Dashboard Dekho" aur "Upload Panel" dono buttons
- Upload page → "View Dashboard" link (upar right mein)
- Dashboard page → "+ Upload New Report" link (upar right mein)

### Test karne ka tarika
1. Saari files GitHub pe upload karo (niche list dekho)
2. Live link kholo → login karo
3. "Dashboard Dekho" dabao
4. Jo report pehle save ki thi, wo dikhni chahiye — numbers, bars, district table sab

---

## Step 5 — Admin email set karna + Security Rules (ZAROORI, abhi karna hai)

Ab sirf **ek hi Gmail account** upload/edit kar payega, baaki sab sirf **dekh** payenge
(bina login ke bhi). Isko kaam karne ke liye 2 jagah chhoti si setting karni hai:

### 5a. `upload.html` mein apna admin email daalo
File mein ye line dhundo (near the top of the `<script>` section):
```js
const ADMIN_EMAIL = "YOUR_ADMIN_EMAIL@gmail.com";
```
Isko apne asli Gmail address se replace karo — jis email se aap login karte ho, wahi daalna
(jaise `rituraj11.something@gmail.com`). Yahi ek email upload panel use kar payega.

### 5b. Firestore Security Rules update karo
1. Firebase Console → **Databases & Storage → Firestore Database → Rules** tab
2. Jo bhi code wahan hai usko hata ke ye paste karo (apna email yahan bhi daalna, upload.html wala hi):
```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /reports/{document} {
      allow read: if true;
      allow write: if request.auth != null
                   && request.auth.token.email == "YOUR_ADMIN_EMAIL@gmail.com";
    }
  }
}
```
3. **Publish** dabao

Iske baad:
- `view-reports.html` **koi bhi, bina login ke** khol sakta hai
- `upload.html` sirf tabhi kaam karega jab aap us admin email se login karo — koi aur login
  karega to usko "Access Denied" dikhega (upload nahi kar payega, par dashboard dekh sakta hai)

### Test karne ka tarika
1. Dono jagah apna email daal ke, saari files GitHub pe upload karo
2. Incognito window mein `view-reports.html` kholo (bina login ke) — data dikhna chahiye
3. Apne Gmail se `index.html` → login → "Go to Upload Panel" → kaam karna chahiye
4. Kisi doosre Gmail se login karke test karo — "Access Denied" dikhna chahiye

---

## 4 Report Types (ab sab ek saath available hain)

Upload Panel mein ab ye 4 options milenge:
1. **Cards Generated (District-wise)**
2. **Eligible Beneficiaries (District-wise)**
3. **Pending Cards (District-wise)**
4. **Period Growth Report** — isme ek extra "Period Start Date" field bhi hai (jaise 15-Jul se
   01-Oct tak wali reports)

Dashboard (`view-reports.html`) mein bhi yahi 4 options dropdown mein milenge — jo bhi save
kiya hoga wahi dikhega.

---

## Files is folder mein

- `login.html` / `index.html` — Google Sign-In page, admin login ke liye (dono same hain)
- `upload.html` — Report upload panel — **admin-only**, 4 report types
- `view-reports.html` — **Public** live dashboard — koi bhi bina login dekh sakta hai
- `README.md` — ye file
