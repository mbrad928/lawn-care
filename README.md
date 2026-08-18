# Lawn Care

Lawn care tools for 7823 Rockburn Drive (Ellicott City, MD — USDA zone 7a). Uses Open-Meteo for weather and Firebase Firestore to sync watering logs across devices.

- **`index.html`** — watering recommendation tool. Tracks rainfall vs. a weekly goal, lets you log manual watering and soil-moisture readings, and syncs across devices via Firestore.
- **`calendar.html`** — full-year lawn care calendar (mowing height, fertilizer, pre-emergent, aeration/overseeding, fungicide/brown-patch monitoring) tuned for a tall fescue / fine fescue lawn with some Kentucky bluegrass, plus a dedicated multi-step plan for spot-treating and removing Bermuda grass patches. Pulls live soil temperature and forecast data from Open-Meteo to flag when you're actually in a trigger window (e.g. soil warm enough for pre-emergent, or elevated brown-patch risk this week) rather than just showing static dates. Grass profile and location constants live at the top of the `<script>` block if either ever changes.

## Firebase Setup (one-time)

1. Go to [console.firebase.google.com](https://console.firebase.google.com) and create a new project.
2. In the project, click **Firestore Database → Create database**. Choose **production mode** and pick a region close to you (e.g. `us-east1`).
3. Under **Firestore → Rules**, paste and publish:
   ```
   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {
       match /watering-logs/shared {
         allow read, write: if true;
       }
     }
   }
   ```
4. Go to **Project Settings → Your apps → Add app → Web**. Register an app (any nickname). Copy the `firebaseConfig` object values.
5. Open `index.html` and fill in your values in the `FIREBASE_CONFIG` block near the top of the script:
   ```js
   const FIREBASE_CONFIG = {
     apiKey:    "...",
     authDomain: "...",
     projectId:  "...",
   };
   ```

That's it. Open `index.html` in any browser and watering logs will sync across all your devices in real time.

## Hosting (optional)

Deploy `index.html` to any static host (Firebase Hosting, GitHub Pages, Netlify, etc.) so it's accessible from your phone without needing a local file.
