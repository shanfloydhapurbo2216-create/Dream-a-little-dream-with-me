# Vibe coding my way into a beautiful heart



A single-page website — a playlist you click to play, quotes that fade in, and the reasons
why. Dark, coral-accented, with falling petals and a glass music player.

Built with **plain HTML + CSS + vanilla JavaScript**. No frameworks, no build step, no npm install.

> Inspired by the interface of [`flymuel/surat-untukmu`](https://github.com/flymuel/surat-untukmu),
> rebuilt in English with three sections and a fixed navigation bar.

---

## Sections

| # | Section | What's in it |
|---|---------|--------------|
| 1 | **A Little Dream of Me (For Suha)** | Title + round cover art + 5 clickable covers (click one to change the song) |
| 2 | **Notes from the Underground, Letters to Suha** | Quote boxes with a coral left border, fade in one after another |
| 3 | **In Every Version of You** | Six reason cards — "Your Smile", "Your Soul", … |
| — | Closing | "You Are Loved Beyond Words" + the signature line at the end |

Plus: a tap-to-open gift cover screen, a "No 😜" button that runs away from the cursor, falling petals,
and a fixed music widget (⏮ ⏸ ⏭ + seek bar + timers) that auto-advances to the next song.

---

## File structure

```
index.html      ← page structure: sections, playlist tiles, quotes, cards
style.css       ← all styling (colours, layout, animations, mobile rules)
script.js       ← player, petals, gift popup, section nav, fade-ins

lagu1.mp3 … lagu5.mp3     ← the five songs
cover1.jpg … cover5.jpg   ← cover art for each song
bg.jpg                    ← background photo behind the dark gradient

README.md · LICENSE · .gitignore
```

Everything sits in the **same folder** — there are no subfolders, which keeps the file paths short:
`src="lagu1.mp3"`, `src="cover1.jpg"`, `url('bg.jpg')`.

**What you'll normally edit:** `script.js` (song list), `index.html` (text), `style.css` (colours).

---

## Quick start

Just open `index.html` in your browser.

For a local server (recommended — some browsers are strict about local audio):

```bash
python3 -m http.server 8000     # then open http://localhost:8000
```

---

## How to edit

### Change an existing song

Two places, keep them matching:

1. **`script.js`** — the list at the top (used by ⏮ ⏭ and auto-play-next):
   ```js
   const playlist = [
       { src: 'lagu1.mp3', title: 'Hanya Untuk-Mu', artist: 'Ten2Five', cover: 'cover1.jpg' },
       { src: 'lagu2.mp3', title: 'Aku Milikmu',    artist: 'Dewa19',   cover: 'cover2.jpg' },
       // ...
   ];
   ```
2. **`index.html`** — the matching tile's `onclick`, e.g.
   ```html
   <div class="photo-item" onclick="changeSong('lagu1.mp3', 'Hanya Untuk-Mu', 'Ten2Five', 'cover1.jpg')">
   ```
   …and the `<img src="cover1.jpg">` plus the two caption lines right underneath it.

The easiest way to change a song: **keep the same filenames** (`lagu1.mp3`, `cover1.jpg`) and just
replace the files on GitHub. Then you only edit the *title* and *artist* text.

### Add a 6th song

1. Upload `lagu6.mp3` and `cover6.jpg` to the repo (same folder, no subfolder).
2. Add a line to the `playlist` array in `script.js`:
   ```js
   { src: 'lagu6.mp3', title: 'Your Song', artist: 'Your Artist', cover: 'cover6.jpg' },
   ```
3. Copy a tile in `index.html` inside `<div class="playlist-grid">` and change its four values to match.
   (The grid is 5 columns, so the 6th tile simply wraps onto a second row — that's fine.)

### Add a quote

Inside `<section id="quotes">` → `<div class="quote-stack">`, copy a block:

```html
<div class="quote-box">
    <p class="serif quote-text">"Your quote goes here."</p>
    <p class="quote-author">— who said it</p>
</div>
```

They fade in with increasing delay automatically (see the `section.in-view > *:nth-child(n)` rules in
`style.css` — there are 7, add more if you add many elements to one section).

### Add a reason card

Inside `<section id="reasons">` → `<div class="reasons-grid">`:

```html
<div class="card"><div class="card-icon">🌙</div><h3 class="serif">Your Calm</h3><p>One sentence about it.</p></div>
```

3 columns on desktop, 2 on phones.

### Change the colours

Top of `style.css`:

```css
:root {
    --bg-dark:    #1f0b17;   /* cover screen + popup background */
    --bg-card:    rgba(255, 255, 255, 0.04);
    --text-main:  #fce4ec;   /* body text */
    --text-muted: #d4a5b9;   /* small uppercase labels */
    --accent:     #e57373;   /* coral: buttons, borders, glows */
}
```

### Change any text

It's all plain HTML in `index.html` — search for a phrase and type over it.

### Remove the intro cover screen (open straight to the page)

Delete the `<div id="cover-screen">…</div>` and `<div id="popup-overlay">…</div>` blocks, then in
`style.css` change:

```css
#main-content { display: none; opacity: 0; ... }   /*  →  */
#main-content { display: block; opacity: 1; }
```

Heads-up: browsers block music until the visitor interacts with the page, so without the tap-to-open
screen the song won't start until someone presses ▶.

---

## Deploy to GitHub Pages

**Browser only (no terminal):**

1. Repo → **Add file → Upload files** → drag in the files you changed → **Commit changes** (uploading a
   file with the same name overwrites the old one).
2. Repo → **Settings → Pages** → *Deploy from a branch* → Branch **main**, folder **/ (root)** → **Save**.
3. Wait 1–2 minutes. Your site is live at:
   `https://YOUR-USERNAME.github.io/YOUR-REPO/`

Every later change updates the site about a minute after you commit — no re-deploy needed.

**With git:**

```bash
git add .
git commit -m "update songs"
git push
```

---

## Notes & limits

- **Audio file sizes.** GitHub warns above 50 MB per file and blocks above 100 MB. A 4-minute MP3 at
  128–192 kbps is ~4–6 MB, so a handful is fine. For many songs, host the MP3s elsewhere (Dropbox direct
  link, Cloudflare R2, Internet Archive) and put the full `https://…` URL in `src:` instead.
- **Copyright.** Only upload music you have the right to use publicly. The MP3s here are short
  placeholders I generated — replace them with your own.
- **Petals** are created in `script.js` (`for (let i = 0; i < 35; i++)` — change 35 to taste), styled by
  the `.petal` rule in `style.css`.
- **Mobile:** the music widget moves to the bottom-center and the wordmark hides under 520px.
- The **"No 😜" button** running away is intentional (`moveButton()` in `script.js`) — delete the
  `btnNo.addEventListener(...)` block if you'd rather it behave.

---

## License

Code: MIT — see [LICENSE](LICENSE). Replace `[Your Name]` in that file.
Content (your songs, photos, words): yours.
