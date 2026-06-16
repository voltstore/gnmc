# gnmc — personal bio page

A single-file static personal bio page (à la guns.lol / gons.lol style) with a hidden admin dashboard.

## How it works

- Open the site → click anywhere to enter (unlocks audio autoplay)
- Public profile shows: avatar, name, tagline, bio, social links, background music
- **Secret access:** click the avatar **10 times** in 4 seconds → enter the secret code
- Default secret code: `1234` (change it from the dashboard → Settings)

## Dashboard tabs

- **Profile** — name, tagline, bio, avatar upload
- **Links** — add/edit/reorder/delete social links (13 platforms supported)
- **Themes** — 10 preset themes + custom theme editor + background image/effect
- **Audio** — background music upload, volume, autoplay, loop
- **Settings** — change secret code, export JSON backup, reset

## Stack

- Single `index.html` file (no build step)
- React 18 + Tailwind CSS (via CDN)
- Firebase Firestore (config) + Firebase Storage (media) — project `imam-warsh`
- Falls back to `localStorage` when offline

## Deployment

This site is hosted on **GitHub Pages**.

## Firebase Security Rules (required!)

Go to [Firebase Console → Firestore → Rules](https://console.firebase.google.com/project/imam-warsh/firestore/rules) and paste:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /sites/gnmc {
      allow read: if true;
      allow write: if true; // client-side hash validation; ok for personal site
    }
  }
}
```

For Storage rules ([here](https://console.firebase.google.com/project/imam-warsh/storage/rules)):

```
rules_version = '2';
service firebase.storage {
  match /b/{bucket}/o {
    match /{allPaths=**} {
      allow read: if true;
      allow write: if request.resource.size < 20 * 1024 * 1024;
    }
  }
}
```

## License

Private.
