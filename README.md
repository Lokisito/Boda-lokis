[README.md](https://github.com/user-attachments/files/29443612/README.md)
# Quédate el fin de semana — invitación animada

A self-contained animated wedding invite (the dog orbits the couple) that you
share on WhatsApp as a **link**. Plain HTML + CSS + vanilla JS — no build step,
no framework, no `support.js`. Hosts for free anywhere.

```
QuedateAnimada/
├── index.html        ← the whole invite (open it in any browser)
└── assets/
    ├── og.jpg        ← static preview card shown in the WhatsApp bubble
    ├── dog.png       ← (you add) transparent running dog
    └── couple.png    ← (you add) transparent couple
```

---

## The one thing to understand about WhatsApp

**WhatsApp never animates a link preview.** When you paste a link, WhatsApp shows
a single *static* image (the Open Graph image, `og.jpg`). That is by design and
there is no way around it on any platform.

The animation plays the moment someone **taps the link** and the page opens in
their browser. So the flow is:

> You paste the link → WhatsApp shows a pretty static card (`og.jpg`) →
> guest taps it → the animated invite opens. Works on any phone, any browser,
> no app or account needed. ✅

If you instead want motion *inside the chat bubble itself*, the only option is to
send a **GIF or short MP4** (see "Bonus" at the bottom) — but that's lower
quality and not a real page. The link is the good experience.

---

## Step 1 — add the two illustrations (optional but recommended)

The page works right now with simple placeholder silhouettes. To use the real
art, drop these two **transparent PNGs** into `assets/` (same names):

- `assets/dog.png` — the running dog, transparent background
- `assets/couple.png` — the couple, transparent background

These are the exact files from your Claude Design project
("Wedding invite proposal" → `assets/dog.png`, `assets/couple.png`). Export them
from there and copy them in. Nothing else changes.

> They weren't bundled automatically because the design API caps file downloads
> at 256 KB and both PNGs are larger.

## Step 2 — personalize two links (optional)

Open `index.html` and edit:

- **"Reserva aquí" button** — replace `52XXXXXXXXXX` with the host's WhatsApp
  number (country code, no `+`, e.g. `527771234567`). It opens a chat with a
  prefilled message. Or change the whole `href` to a Google Form / your bank
  details page.
- The address already links to Google Maps — adjust if the venue differs.

## Step 3 — host it for free

Pick ONE. All are free and give a public `https://` link you can paste in WhatsApp.

### Option A — Netlify Drop (easiest, ~1 min, no terminal)
1. Go to **https://app.netlify.com/drop**
2. Drag the whole `QuedateAnimada` **folder** onto the page.
3. You instantly get a URL like `https://quedate-lokis.netlify.app`.
4. (Free account lets you rename it and keep it permanent.)

### Option B — GitHub Pages (most permanent, stable URL)
1. Create a repo, e.g. `quedate-animada`, and upload these files.
2. Repo **Settings → Pages → Source: main / root → Save**.
3. URL becomes `https://<your-username>.github.io/quedate-animada/`.

### Option C — Cloudflare Pages
Similar to Netlify — connect the repo or drag-and-drop, get a `*.pages.dev` URL.

## Step 4 — make the WhatsApp preview look right

For the static preview card to show, edit the 4 `__BASE_URL__` placeholders near
the top of `index.html` and replace them with your real URL (no trailing slash):

```html
<meta property="og:url"   content="https://your-real-url/">
<meta property="og:image" content="https://your-real-url/assets/og.jpg">
```

Then test the preview at **https://www.opengraph.xyz** (paste your URL). WhatsApp
caches previews aggressively — if you change `og.jpg` later, the old one may stick
for a while; adding `?v=2` to the link forces a refresh.

> Want the preview card to show the real couple instead of the text card?
> Replace `assets/og.jpg` with a 1200×630 screenshot of the rendered invite.

---

## How the animation works (so you can tweak it)

- The invite is designed at a fixed **1024×1536**; a few lines of JS in
  `index.html` scale that whole card down to fit any phone width, so the layout
  and the dog's orbit stay pixel-perfect at every size.
- The dog follows an ellipse around the couple (`period = 7` seconds per lap),
  growing/shrinking and passing in front of / behind them. All the orbit numbers
  live in the `loop()` function — change `period`, `a`, `b` to taste.
- Respects `prefers-reduced-motion` (parks the dog) and pauses when the tab is
  hidden to save battery.

## Bonus — a GIF/MP4 to send directly in chat (in-bubble motion)
If you also want a clip that animates inside WhatsApp:
```bash
# record ~7s (one full lap) of the page, then:
ffmpeg -i capture.mov -vf "fps=20,scale=600:-1" quedate.gif      # GIF
ffmpeg -i capture.mov -vf "scale=720:-1" -t 7 quedate.mp4        # MP4 (better)
```
Send that as a normal attachment. Keep the link too — it's the nicer experience.
