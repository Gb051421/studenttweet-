# StudentTweet

StudentTweet is a lightweight Twitter-like social app designed for students.  
Built with **React** (Vite) and **Firebase** for authentication, database, storage, and hosting.

## Features
- Google & email authentication
- Post short messages with optional images
- Real-time feed with newest posts first
- Like posts
- Basic profile (display name, avatar)

## Tech Stack
- **React** (Vite)
- **Firebase Authentication**
- **Firestore** (real-time NoSQL database)
- **Firebase Storage** (for images)
- **Tailwind CSS** (optional styling)
- **Firebase Hosting**

## Setup Instructions

### 1. Clone the repository
```bash
git clone https://github.com/YOUR_USERNAME/studenttweet.git
cd studenttweet
```

### 2. Install dependencies
```bash
npm install
```

### 3. Configure Firebase
- Create a Firebase project: https://console.firebase.google.com/
- Enable **Authentication** (Email/Password + Google)
- Create a **Firestore Database** (test mode for development)
- Enable **Firebase Storage**
- Copy your Firebase config object and paste it into `src/App.jsx`:

```javascript
const firebaseConfig = {
  apiKey: "YOUR_API_KEY",
  authDomain: "YOUR_PROJECT.firebaseapp.com",
  projectId: "YOUR_PROJECT_ID",
  storageBucket: "YOUR_PROJECT.appspot.com",
  messagingSenderId: "YOUR_SENDER_ID",
  appId: "YOUR_APP_ID"
};
```

### 4. Run locally
```bash
npm run dev
```

### 5. Build for production
```bash
npm run build
```

### 6. Deploy to Firebase Hosting
```bash
npm install -g firebase-tools
firebase login
firebase init hosting
firebase deploy
```

## Firestore Rules (Example)
```plaintext
service cloud.firestore {
  match /databases/{database}/documents {
    match /users/{userId} {
      allow read: if true;
      allow write: if request.auth != null && request.auth.uid == userId;
    }
    match /posts/{postId} {
      allow read: if true;
      allow create: if request.auth != null && request.resource.data.authorId == request.auth.uid;
      allow update, delete: if request.auth != null && resource.data.authorId == request.auth.uid;
    }
    match /likes/{likeId} {
      allow create, delete: if request.auth != null && (request.resource.data.userId == request.auth.uid || resource.data.userId == request.auth.uid);
      allow read: if true;
    }
    match /follows/{doc} {
      allow write: if request.auth != null && (request.resource.data.followerId == request.auth.uid);
      allow read: if true;
    }
  }
}
```

## License
This project is open-source and free to use for educational purposes.
