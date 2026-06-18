# Lawn Care

Lawn watering recommendation tool for 7823 Rockburn Drive. Uses Open-Meteo for weather and Firebase Firestore to sync watering logs across devices.

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
