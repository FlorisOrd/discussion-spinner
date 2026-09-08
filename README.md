# Discussion Spinner

A one-page question spinner for hosting **AI & society** discussion meetings. Hit **Spin**, the
questions blur past like a slot machine for a second and a half, and one lands on screen — large,
centred, and readable from the back of a room.

No backend, no login, no analytics, no build step. Two files do the work: `index.html` and
`questions.json`.

## How it behaves

- **Spin** draws a question at random from the ones not yet used this session.
- **Skip this one** re-spins and puts the skipped question back in the pool, so it can still come up later.
- The counter in the top-right shows how many of the list you've been through. Once every question has
  been used, the list reshuffles and starts over (it won't repeat the question you just had).
- **Space**, **Enter**, or the **right-arrow / page-down** keys also spin — so a presentation clicker works.
- "Used" is per browser session. Reload the page and the tracker resets.

## Running it

**On your phone or a projector — just visit the deployed URL.** Nothing to install.

**Locally**, it needs to be served over HTTP, because browsers refuse to let a page opened with
`file://` read `questions.json`. From this folder:

```bash
python -m http.server
```

Then open <http://localhost:8000>. (`npx serve` works just as well if you'd rather use Node.)

## Adding your own questions

Open `questions.json`. It's a plain array of strings — one question per entry:

```json
[
  "Should schools teach students to use AI, or teach them to work without it first?",
  "Your new question goes here?"
]
```

Rules of the format:

- Every entry is a **double-quoted string**, and every entry except the last is followed by a **comma**.
- Use `\"` for a quotation mark inside a question, and `—` or a literal `—` for an em dash.
- Keep them short. Anything over ~25 words starts shrinking on screen and stops being readable
  from across a room.
- The app takes the list as-is, so there's no limit on how many you add — the counter adjusts itself.

Save the file, reload the page, and the new questions are in the rotation. If you've deployed to
GitHub Pages, commit and push and the live site picks them up within a minute or so.

If the page says it couldn't load the questions, `questions.json` almost certainly has a syntax
error — a missing comma or a stray trailing one. Paste it into any JSON validator to find the line.

## Deploying to GitHub Pages

1. Create an empty repository on GitHub (no README, no `.gitignore` — this repo already has them).
2. Point this folder at it and push:

   ```bash
   git remote add origin https://github.com/<your-username>/discussion-spinner.git
   git push -u origin main
   ```

3. In the repository, go to **Settings → Pages**, set **Source** to *Deploy from a branch*, pick
   branch `main` and folder `/ (root)`, and save.
4. A minute later the site is live at `https://<your-username>.github.io/discussion-spinner/`.

Every later push to `main` redeploys automatically.

## Changing the look

All the styling lives in the `<style>` block at the top of `index.html`. The colours are CSS
variables in `:root` — change `--accent` to recolour the button, the highlight, and the focus ring in
one go. Question size is a `clamp()` on `#question`, so it scales with the screen; raise the middle
value if you want it bigger on a projector.
