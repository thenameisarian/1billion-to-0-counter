# 1 Billion to 0 — Live Counter

The self-updating countdown that powers **https://counter.mustafaarian.com**.

The page reads the channel's YouTube uploads live, auto-detects every daily Short
(by its 📉 / "1BillionTo0" marker), sums their likes and comments, and subtracts
from 1,000,000,000 in real time. Post a new day and it appears on its own — no edits,
no redeploys.

The site itself is just `index.html` (one self-contained file).

---

## Auto-deploy: how a push becomes a live update

Every push to `main` runs `.github/workflows/deploy.yml`, which publishes `public/`
to the existing Cloudflare Pages project **`1billionto0`** (the one already serving
`counter.mustafaarian.com`). Edit the file, commit, done.

### One-time setup (about 3 minutes)

You only do this once. After that, every change auto-deploys.

1. **Create a Cloudflare API token**
   - Go to https://dash.cloudflare.com/profile/api-tokens → *Create Token*.
   - Use the **"Edit Cloudflare Workers"** template, OR create a custom token with
     permission: **Account → Cloudflare Pages → Edit**.
   - Create the token and copy it (you only see it once).

2. **Add three secrets to this GitHub repo**
   - Repo → *Settings* → *Secrets and variables* → *Actions* → *New repository secret*.
   - `CLOUDFLARE_API_TOKEN` = the token you just copied.
   - `CLOUDFLARE_ACCOUNT_ID` = `9e8274de7401e66fcba43a92ebfd8112`
   - `YT_API_KEY` = your YouTube Data API v3 key (the `AIza...` one).
     The repo stores only a `__YT_API_KEY__` placeholder; the real key is injected
     into the page at deploy time, so it never lives in the repo.

3. **Trigger the first deploy**
   - Make any small commit (or open the *Actions* tab → *Deploy to Cloudflare Pages*
     → *Run workflow*). It builds and publishes automatically.

That's it. From now on, editing `public/index.html` and pushing = the live counter updates.

---

## Notes

- Nothing here needs touching for daily content — the counter finds new Shorts by itself.
- To add a weekly subs milestone or a raid bonus, edit the `CONFIG.adjustments` list
  near the top of the `<script>` in `public/index.html`, then commit.
- The counter reads YouTube's public API, so anyone can verify the math.
