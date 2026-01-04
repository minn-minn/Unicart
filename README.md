# 🛒 UniCart

UniCart is a Chrome extension and web dashboard that automatically detects **Add to Cart** actions on shopping websites and saves products into a **personal, cloud-synced cart**.

Each user has their own secure cart, favorites, and product history powered by Firebase.

---

## Features

- Firebase Authentication (Login & Register)
- User profiles with username
- Automatic cart detection (Chrome Extension)
- Cloud-stored cart items (Firestore)
- Favorite products (black → red heart toggle)
- Per-user cart isolation
- Tailwind CSS UI
- Secure Firestore rules

---

## Project Structure

```

unicart/
│
├── web/
│   ├── login.html
│   ├── register.html
│   └── app.html
│
├── extension/
│   ├── manifest.json
│   ├── background.js
│   │
│   ├── content/
│   │   └── addToCart.js
│   │
│   └── popup/
│       ├── popup.html
│       └── popup.js
│
└── README.md

```

---

## Firebase Setup

### Authentication
- Email / Password authentication enabled

### Firestore Database Structure

```

users (collection)
└── {uid} (document)
• email: string
• username: string
• createdAt: timestamp

```
  └── cartItems (subcollection)
       └── {itemId} (document)
            • name: string
            • link: string
            • image: string
            • price: number
            • favorite: boolean
            • createdAt: timestamp
```

```

---

## Firestore Security Rules

```

rules_version = '2';
service cloud.firestore {
match /databases/{database}/documents {

```
match /users/{userId} {
  allow read, write: if request.auth != null
                     && request.auth.uid == userId;

  match /cartItems/{itemId} {
    allow read, write: if request.auth != null
                       && request.auth.uid == userId;
  }
}
```

}
}

```

---

## Page Flow

| Page | Purpose |
|------|--------|
| login.html | User login |
| register.html | User registration |
| app.html | Auth-protected dashboard |

- Unauthenticated users are redirected to `login.html`
- Each user only sees their own cart items

---

## Favorites

- Each product card includes a heart icon
- 🖤 → ❤️ toggles favorite state
- Favorite status is saved per user in Firestore

---

## Chrome Extension Overview

- Detects **Add to Cart** button clicks
- Extracts:
  - Product name
  - Product image
  - Product price
  - Product page link
- Saves data to:
```

users/{uid}/cartItems

```

---

## Tech Stack

- HTML
- Tailwind CSS
- Vanilla JavaScript
- Firebase Authentication
- Firebase Firestore
- Chrome Extensions API

---

## Project Status

In active development

Planned features:
- Favorites page
- Compare list
- Price history tracking
- Improved extension detection logic

---

## 📄 License

This project is intended for educational and personal use.
