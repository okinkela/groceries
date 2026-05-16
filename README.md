# Namirnice

A shared grocery list web app optimised for iPhone, built as a single `index.html` with no framework or build step. Two or more people share the same list in real time by reading and writing a `groceries.json` file directly via the GitHub Contents API.

## How it works

- The list is stored as `groceries.json` in this repository.
- Each device authenticates with a GitHub personal access token (PAT) entered once in the app settings and saved to `localStorage`.
- On load the app fetches the latest list from GitHub. Every 30 seconds it either pushes local changes (if dirty) or pulls the latest version (if clean).
- Pull-to-refresh (swipe down at the top) forces an immediate fetch.
- A local cache in `localStorage` keeps the last known list available offline.

## Features

- **Add items** — type in the search/add bar and tap + or press Enter.
- **Check off / uncheck** — tap the circle; items animate between the *Za kupiti* (to buy) and bought sections with a FLIP transition.
- **12 categories** — each item carries an emoji category icon (produce, meat, dairy, bakery, pantry, drinks, frozen, personal care, household, fish, pharmacy, other). Tap the icon on any item to change its category.
- **Category filter** — tap the cart icon in the add bar to filter the list by category.
- **Live search** — typing in the add bar filters the list in real time with highlighted matches.
- **Swipe left to delete** — swipe an item left to reveal a red delete button.
- **Clear to-buy** — the document icon in the header unchecks all items (moves everything back to bought/inactive state).
- **Share** — the upload icon in the header shares the current to-buy list as plain text via the native Web Share API.
- **PWA-ready** — `apple-mobile-web-app-capable` meta tags, a custom app icon, and safe-area insets make it feel native when added to the iPhone home screen.

## Item data shape

```json
{ "id": "1747123456789", "name": "Jogurt", "checked": true, "category": "dairy" }
```

`checked: true` means the item is **on the to-buy list**; `checked: false` means it is inactive (already bought or not currently needed).

## Setup

1. Fork or use this repository as-is.
2. Open the app URL in Safari on iPhone.
3. Tap the gear icon (⚙️) and enter a GitHub personal access token with `repo` scope.
4. The token is stored only in your device's `localStorage` — it is never sent anywhere except the GitHub API.
5. Add to Home Screen for the best experience.

## Stack

| Layer | Choice |
|---|---|
| UI | Vanilla HTML / CSS / JavaScript |
| Fonts | DM Serif Display + DM Sans (Google Fonts) |
| Storage | `groceries.json` in this GitHub repo |
| Sync | GitHub Contents API (REST) |
| Hosting | GitHub Pages (or any static host) |
