# How to make this your GitHub profile landing page

GitHub has a special rule: **if you create a public repo whose name is exactly your username, its `README.md` renders at the top of your profile page** (`github.com/ghoshpronay18071997`). That repo is often called the "magic repo".

Your username is `ghoshpronay18071997`, so the repo must be named **`ghoshpronay18071997`** — exact, case-sensitive-ish, no extra characters.

---

## Step 1 — Create the magic repo

Go to <https://github.com/new> and set:

| Field | Value |
| --- | --- |
| Repository name | `ghoshpronay18071997` |
| Visibility | **Public** (required — private repos don't render on the profile) |
| Initialize with README | **No** (leave unchecked, we already have one) |

When the name matches your username GitHub shows a little banner: *"You found a secret! ghoshpronay18071997/ghoshpronay18071997 is a ✨special✨ repository."* That's the confirmation you got it right.

## Step 2 — Push this folder

From `C:\Work\Profile\ghoshpronay18071997`:

```bash
git init -b main
```

```bash
git add .
```

```bash
git commit -m "feat: profile README"
```

```bash
git remote add origin https://github.com/ghoshpronay18071997/ghoshpronay18071997.git
```

```bash
git push -u origin main
```

Visit <https://github.com/ghoshpronay18071997> — the README is now your landing page.

## Step 3 — Let the Actions run once

Three workflows ship with this repo. Until each has run at least once, the images they produce will show as broken.

| Workflow | What it produces | First run |
| --- | --- | --- |
| `profile-summary-cards.yml` | The four stat cards under "GitHub in numbers" | Runs on push (it watches its own file), then daily at 18:00 UTC |
| `snake.yml` | The contribution-graph snake, pushed to an `output` branch | Runs on push to `main`, then daily |
| `update-readme.yml` | The "Latest open source" project cards | Runs on push, then every 6 hours |

To trigger them manually: **repo → Actions tab → pick the workflow → "Run workflow"**.

If the Actions tab says workflows are disabled, enable them under **Settings → Actions → General → Allow all actions**, and make sure **Workflow permissions** is set to **Read and write permissions** (the workflows commit back to the repo).

Give the snake workflow a couple of minutes — it creates a new branch called `output`. That branch is meant to exist; don't delete it.

## Step 4 — Optional secrets

Both are optional; everything works without them.

| Secret | Why | Scopes |
| --- | --- | --- |
| `PROFILE_TOKEN` | Makes the stat cards count your **private** repo contributions, not just public ones | `repo`, `read:user` |
| `GH_PAT` | Only needed if you set `SHOW_PRIVATE: 'true'` in `update-readme.yml` — lists private repos as text-only, no links | `repo` |

Add them at **Settings → Secrets and variables → Actions → New repository secret**. Create the token itself at <https://github.com/settings/tokens>.

---

## Things to fill in before you push

Search the README for these:

1. **LinkedIn URL** — line near the bottom is a placeholder (`linkedin.com/in/pronay-ghosh`). Replace it with your actual profile URL. There's a `<!-- TODO -->` comment marking it.
2. **Email** — currently `ghosh.pronay18071997@gmail.com`. Change it if you'd rather route recruiters elsewhere.
3. **Phone number** — deliberately *not* included. Your resume has it, but a public GitHub profile is scraped constantly. Add it only if you want that.

## Customising later

- **Card theme** — the stat cards are generated in ~50 themes into `profile-summary-card-output/<theme>/`. The README points at `2077`. Swap that folder name in the four image URLs to change the look (`dracula`, `nord_dark`, `github_dark`, `tokyonight`, etc.).
- **Colour scheme** — the purple/indigo palette is `0F0C29 → 302B63 → 24243E` with accent `7C4DFF` / `A78BFA`. Find-and-replace those hex codes to re-theme the whole page.
- **Project descriptions** — once you have public repos, add curated one-liners to the `OVERRIDES` object in `scripts/update-readme.js` so the auto-generated cards read well instead of using GitHub's raw description.
- **A blog section** — if you start writing on Medium/Dev.to, add a `blog-posts.yml` workflow using `gautamkrishnar/blog-post-workflow` and drop `<!-- BLOG-POST-LIST:START -->` / `<!-- BLOG-POST-LIST:END -->` markers into the README.

## Don't auto-format the README

`.prettierignore` and `.vscode/settings.json` are here on purpose. A Markdown formatter will flatten the badge rows and can strip the HTML comment markers the Actions rely on, which quietly breaks the auto-updating sections.
