# bldgtyp.com

Source for the BLDGTYP company homepage. Deploys automatically to Dreamhost via FTP on every push to `main`.

## Repo structure

```
homepage/
├── .github/workflows/deploy.yml   ← GitHub Actions FTP deploy
├── site/                           ← Everything in here gets uploaded to Dreamhost
│   ├── index.html                  ← The homepage
│   └── assets/
│       ├── hero-drafting.png
│       ├── detail-foundation.png
│       └── detail-wall-section.png
└── README.md
```

Only the contents of `site/` are deployed. Existing server content (like `/Proj_*` report directories) is **never touched or deleted**.

## Setup (one-time)

### 1. Create the GitHub repo

```bash
gh repo create bldgtyp/bldgtyp-site --private
cd bldgtyp-site
git init
git add .
git commit -m "Initial homepage"
git remote add origin git@github.com:bldgtyp/bldgtyp-site.git
git push -u origin main
```

### 2. Add FTP secrets to the repo

Go to **github.com/bldgtyp/bldgtyp-site → Settings → Secrets and variables → Actions** and add three repository secrets:

| Secret     | Value                                                                                          |
| ---------- | ---------------------------------------------------------------------------------------------- |
| `FTP_HOST` | Your Dreamhost FTP hostname (e.g. `ftp.bldgtyp.com` or check Dreamhost panel → Manage Domains) |
| `FTP_USER` | Your Dreamhost FTP username for bldgtyp.com                                                    |
| `FTP_PASS` | The FTP password                                                                               |

To find your FTP credentials in Dreamhost:

1. Log into panel.dreamhost.com
2. Go to **Manage Websites** → click your domain
3. Look under **Login Info** for the username and server

### 3. Test the deploy

Push any change to `main` and check **Actions** tab on GitHub. The workflow will upload only new/changed files without disturbing existing content on the server.

You can also trigger a deploy manually from the Actions tab using "Run workflow".

## How it works

The GitHub Action uses `lftp mirror` with these critical flags:

- `--only-newer` — only uploads files that have actually changed
- `--no-remove-other` — **never deletes** files on the server that aren't in this repo (protects `/Proj_*`, `/downloads`, etc.)

This means you can safely keep all your existing report directories and downloads on Dreamhost while deploying homepage updates from GitHub.

## Local development

Just open `site/index.html` in a browser. No build step needed — it's plain HTML/CSS/JS.

For a local server (optional, for proper asset paths):

```bash
cd site
python3 -m http.server 8000
# Open http://localhost:8000
```

## Design system

This site uses the [BLDGTYP Design System](https://bldgtyp.github.io/branding/). Tokens are inlined in the HTML for zero-dependency deployment, but the canonical source is:

- **CSS tokens:** `https://bldgtyp.github.io/branding/tokens/tokens.css`
- **JSON tokens:** `https://bldgtyp.github.io/branding/tokens/tokens.json`
