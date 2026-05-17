# Namirnice

A shared grocery list PWA optimised for iPhone, built as a single `index.html` with no framework or build step. Two or more people share the same list in real time via the GitHub Contents API.

## How it works

- The list is stored as `groceries.json` in this repository.
- Each device authenticates with a GitHub personal access token (PAT) entered once in Settings and saved to `localStorage`.
- On load the app fetches the latest list from GitHub. Every 30 seconds it either pushes local changes (if dirty) or pulls the latest version (if clean).
- Pull-to-refresh (swipe down at the top) forces an immediate sync.
- Returning to the app (Page Visibility API) triggers an immediate fetch.
- A `localStorage` cache keeps the last known list available offline.

## Features

### List management
- **Add items** — type in the search/add bar and tap + or press Enter.
- **Check off / uncheck** — tap the circle; items animate between the *Za kupiti* (to-buy) and inactive sections with a FLIP transition.
- **Quantity** — tap the quantity badge or long-press an item to open the quantity picker. Supports both numeric stepping and free-text (e.g. *500g*, *1.5kg*). Setting quantity to 0 marks the item as not-to-buy. All items show a quantity badge; inactive items without a set quantity show a dimmed default.
- **Edit item name** — long-press the item name to edit it in place.
- **Photo** — tap the camera icon on an item to attach a photo (compressed to 400 px JPEG). Tap again to view or remove it.
- **Swipe left to delete** — swipe an item left to reveal a red delete button.
- **Clear to-buy** — the document icon in the header moves all items back to inactive, with a confirmation dialog showing the count.
- **Share** — the upload icon in the header sends the current to-buy list (with quantities) via WhatsApp or SMS deep link. Configure the recipient phone number and preferred app in Settings.

### Notifications
- **Changes popup** — when the app comes to the foreground and another user has added new items, a sheet appears listing what was added.
- **Share via WhatsApp / SMS** — in Settings, enter a recipient phone number and choose WhatsApp or SMS. The share button composes a pre-filled message with the to-buy list.

### Search & filter
- **Live search** — typing in the add bar filters the list in real time with highlighted matches. Croatian diacritics are handled (searching *c* matches *č*, *ć*, etc.).
- **Category filter** — tap the cart icon in the add bar to filter by category.
- **Sort** — cycle between *Kategorija → Za kupiti → Abeceda* and *Za kupiti → Kategorija → Abeceda* in Settings.

### Categories
- **Emoji categories** — every item carries a category icon. Tap the icon on an item to reassign it.
- **Per-user icon overrides** — each user can pick their own emoji for any category; the override is stored locally and does not affect other users. The shared default icon lives in `groceries.json`.
- **Manage categories** — in Settings → *Ikone kategorija*, add new categories, delete existing ones, or drag to reorder them (iPhone icon-style drag). Changes are buffered until you tap *Spremi*.
- **Per-user order** — category order is stored locally; the server holds the canonical list.

### PWA
- `apple-mobile-web-app-capable` meta tags, a custom app icon, and safe-area insets make it feel native when added to the iPhone home screen.

## Data format

`groceries.json` holds both the category list and the items:

```json
{
  "categories": [
    { "id": "produce", "icon": "🍋", "label": "Voće i povrće" }
  ],
  "items": [
    { "id": "1747123456789", "name": "Jogurt", "checked": true, "category": "dairy", "qty": 2 },
    { "id": "1747123456790", "name": "Losos", "checked": false, "category": "fish", "qty": "500g" }
  ]
}
```

- `checked: true` — item is on the to-buy list; `checked: false` — inactive (already bought or not needed).
- `qty` — optional. Number or free-text string. Omitted when not set. `0` means not-to-buy.
- `photo` — optional. Raw base64-encoded JPEG (no `data:` prefix). Stored in JSON; large photos count toward the 1 MB GitHub file limit.
- Category `icon` in the JSON is the shared default. Per-user overrides are stored in `localStorage` only.

## Setup

1. Fork or use this repository as-is.
2. Open the app URL in Safari on iPhone.
3. Tap the gear icon and enter a GitHub personal access token with `repo` scope.
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
