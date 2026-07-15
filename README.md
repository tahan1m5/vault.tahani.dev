# vault

A calm, PIN-locked personal dashboard — one glassy home for every site, server, and
tool you own. No build step, no dependencies, no backend. Just three HTML files and a
logo.

<p align="center">
  <em>Loading screen → PIN gate → your world, on a scrollable wheel.</em>
</p>

---

## Features

- **Scrollable app wheel** — a curved, spring-physics launcher you drive with the
  mouse wheel, drag, arrow keys, or touch. Each app opens a soft "glass" info card.
- **PIN gate** — a phone-style keypad (with an erase key and a lockout after 3 wrong
  tries) stands between the loading screen and the dashboard.
- **Three themes** — pick one in **Settings**, and it's remembered on your device:
  -  **Meadow** — the fresh green default
  -  **Noir** — elegant, minimal dark
  -  **Blossom** — white with soft pink
- **Themed end to end** — the loading screen, PIN pad, and dashboard all follow your
  chosen theme, applied before first paint so there's no flash.
- **Playful, hand-lettered headings** in [Kranky](https://fonts.google.com/specimen/Kranky),
  calm [Quicksand](https://fonts.google.com/specimen/Quicksand) body text, floating
  pastel blobs, and twinkling sparkles.
- **Keyboard friendly** — `↑` `↓` `←` `→` to browse, `Home`/`End` to jump, `Enter`
  to launch.
- **Zero dependencies** — plain HTML, CSS, and vanilla JavaScript. Fonts are the only
  external resource.

---

## Structure

| File             | What it does                                                        |
| ---------------- | ------------------------------------------------------------------- |
| `index.html`     | Animated loading screen; hands off to the PIN gate.                 |
| `auth.html`      | 4-digit PIN keypad. On success, unlocks the dashboard.              |
| `dashboard.html` | The main event — the app wheel, info cards, and Settings.           |
| `Assets/`        | The stamp logo shown on the loading screen.                         |

The flow is: **`index.html` → `auth.html` → `dashboard.html`**.

---

## Getting started

Because everything is static, you can just open `index.html` in a browser — but themes
are saved in `localStorage`, which behaves best when the pages are served from one
origin. The simplest way:

```bash
# from the project folder
python3 -m http.server 8000
# then visit http://localhost:8000
```

Then enter the PIN to reach the dashboard.

---

## Customizing

### Add or edit your apps

Open `dashboard.html` and find the `APPS` array. Each entry looks like this:

```js
{
    id:'portainer', title:'Portainer', tag:'Self-hosted', icon:'container', color:'#4FA8A8',
    description:'Docker container management for your home server.',
    primary:{ label:'Open Portainer', url:'https://localhost:9443' },
    secondary:{ label:'SSH', url:'ssh://root@homeserver' }   // optional
}
```

| Field         | Meaning                                                        |
| ------------- | -------------------------------------------------------------- |
| `id`          | Unique slug (`settings` renders the theme picker).             |
| `title`       | Big heading on the info card.                                  |
| `tag`         | Small label pill at the top of the card.                       |
| `icon`        | Key from the `ICONS` set (used on the wheel chip).             |
| `color`       | Tint for the wheel chip's glow.                                |
| `description` | A calm sentence or two.                                        |
| `primary`     | The main button: `{ label, url }`.                             |
| `secondary`   | An optional second button.                                     |

Add, remove, or reorder entries freely — the wheel rebuilds itself.

### Change the PIN

The passcode lives in `auth.html`:

```js
const PASS = "2580";
```

### Add a theme

Themes are just CSS-variable overrides. Add a block like
`html[data-theme="yourtheme"] { ... }` in each of the three files, then add an entry to
the `THEMES` array in `dashboard.html`.

---

## A note on the PIN

The PIN is a **friendly gate, not real security** — it lives in client-side JavaScript
and anyone who views the source can read it. It keeps casual eyes out of your start
page; it is not a substitute for authentication on the services it links to.

---