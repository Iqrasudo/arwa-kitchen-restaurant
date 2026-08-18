# Arwa Kitchen — Firebase Backend Setup Guide

Ye guide aapko step-by-step le kar jayegi: Firebase project banana → website ko connect karna → apne GitHub repo mein push karna.

---

## Part 1 — Firebase Project Banayein

1. https://console.firebase.google.com par jayein → **"Add project"**
2. Project ka naam dein (e.g. `arwa-kitchen`) → Continue → (Google Analytics chahें to on/off, matter nahi karta) → **Create project**

### 1a. Firestore Database on karein
1. Left menu mein **Build → Firestore Database** → **Create database**
2. Location choose karein (asia-south1 ya jo bhi closest ho) → **Start in production mode** → Enable

### 1b. Authentication on karein
1. Left menu mein **Build → Authentication** → **Get started**
2. **Sign-in method** tab → **Email/Password** → Enable → Save
3. **Users** tab → **Add user** → apna admin email aur password dalein (ye wahi email/password hoga jo aap Admin Panel mein login karne ke liye use karenge)

### 1c. Web App Register karein (config lene ke liye)
1. Project Overview (home icon) → **</> (Web)** icon par click karein
2. App ka nickname dein (e.g. "Arwa Kitchen Website") → **Register app**
3. Aapko ek `firebaseConfig` object dikhega jaisa ye:
   ```js
   const firebaseConfig = {
     apiKey: "AIzaSy...",
     authDomain: "arwa-kitchen.firebaseapp.com",
     projectId: "arwa-kitchen",
     storageBucket: "arwa-kitchen.appspot.com",
     messagingSenderId: "123456789",
     appId: "1:123456789:web:abc123"
   };
   ```
4. Ye values copy kar lein — agle step mein chahiye honge.

> **Note:** Ye config public/safe hai — ye secret password nahi hai. Real security **Firestore Rules** se aati hai (agla step).

---

## Part 2 — Website File Update Karein

1. `Arwa Kitchen — Complete Website.html` file kholein (kisi text editor mein — Notepad, VS Code, etc.)
2. File ke andar ye block dhoondein (near top of the script section):
   ```js
   window.FIREBASE_CONFIG = {
     apiKey: "YOUR_API_KEY",
     authDomain: "YOUR_PROJECT_ID.firebaseapp.com",
     projectId: "YOUR_PROJECT_ID",
     storageBucket: "YOUR_PROJECT_ID.appspot.com",
     messagingSenderId: "YOUR_SENDER_ID",
     appId: "YOUR_APP_ID"
   };
   ```
3. In saari values ko apne Firebase config se replace kar dein (Part 1c se copy ki hui).
4. File save kar lein.

---

## Part 3 — Firestore Security Rules Apply Karein

1. Firebase Console → **Firestore Database → Rules** tab
2. `firestore.rules` file ka pura content copy karke wahan paste karein
3. **Publish** button dabayein

Ye rules ka matlab:
- Koi bhi customer order place kar sakta hai (login ke bina) ✅
- Sirf logged-in admin hi orders dekh/edit/delete kar sakta hai 🔒

---

## Part 4 — Test Karein

1. File ko browser mein khol kar dekhein (double-click)
2. Koi test order place karein (Self Order ya Monthly Plan se)
3. Logo par click karke Admin Panel kholein → apna email/password (Part 1b wala) daal kar login karein
4. Order turant admin panel mein aana chahiye, saath mein sound + toast notification

---

## Part 5 — GitHub Repo Mein Push Karein

Apne computer ke terminal mein (VS Code terminal ya Command Prompt/PowerShell):

```bash
# Agar repo already clone nahi kiya:
git clone https://github.com/Iqrasudo/Arwa-Kitchen.git
cd Arwa-Kitchen

# Updated file is folder mein copy kar dein (overwrite existing file)
# phir:
git add .
git commit -m "Add Firebase backend, admin panel auth, real-time orders & notifications"
git push
```

Jab `git push` chalega, GitHub aapse login mangega (browser popup ya credential prompt) — apna GitHub account se authenticate kar dein. Token yahan chat mein kabhi type nahi karna.

---

## Part 6 — (Optional) Website ko Live Host Karein

Agar website ko public URL par dalna hai (e.g. `iqrasudo.github.io/Arwa-Kitchen`):

1. GitHub repo → **Settings → Pages**
2. Source: **Deploy from branch** → Branch: `main` → folder: `/ (root)` → Save
3. Kuch minute baad website live ho jayegi us URL par

> Browser notifications sirf **HTTPS** par kaam karti hain (GitHub Pages HTTPS deti hai), file ko seedha double-click kar ke kholne par (`file://`) notifications kaam nahi karengi — is liye Part 4 test mein agar notification na aaye to fikar na karein, wo hosted version par kaam karegi.

---

## Troubleshooting

| Problem | Fix |
|---|---|
| Admin login "Incorrect email or password" | Part 1b mein sahi email/password check karein |
| Orders admin panel mein nahi aa rahe | Firestore Rules publish hui? Config sahi hai? Browser console (F12) mein error dekhein |
| "Connecting…" hi dikh raha hai, "Live" nahi ho raha | Firebase config galat ho sakta hai, ya internet issue |
