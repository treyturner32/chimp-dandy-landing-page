# chimpdandy.com

One-page landing site for Chimp Dandy Labs, hosted free on GitHub Pages.

## Files

| Path | What it is |
|---|---|
| `index.html` | The whole page (styles inline, no build step) |
| `img/bg-wide.*` | Logo art extended sideways, used on landscape screens |
| `img/bg-square.*` | Logo art extended up and sideways, used on tablets and square windows |
| `img/bg-tall.*` | Logo art extended upward, used on phones |
| `img/og-image.jpg` | Link-preview image for iMessage, Slack, social posts |
| `img/apple-touch-icon.png`, `img/icon-512.png`, `favicon.ico` | Browser and home-screen icons |
| `fonts/` | Self-hosted fonts: Anton, DM Serif Display (italic), DM Mono. All SIL Open Font License |
| `CNAME` | Tells GitHub Pages the site lives at chimpdandy.com |
| `.nojekyll` | Tells GitHub Pages to serve files as-is |

To edit text, open `index.html` and change the lines inside `<div class="content">`.

## One-time setup

### 1. Put the site on GitHub (about 2 minutes)

With the GitHub CLI (`brew install gh`, then `gh auth login`), from inside this folder:

```sh
git init -b main && git add -A && git commit -m "Initial site"
gh repo create chimp-dandy-landing-page --public --source=. --push
gh api -X POST repos/treyturner32/chimp-dandy-landing-page/pages -f "source[branch]=main" -f "source[path]=/"
```

Without the CLI: create a public repo named `chimp-dandy-landing-page` at github.com/new, push this folder to it, then in the repo go to **Settings → Pages**, set **Source: Deploy from a branch**, **Branch: main / (root)**, and save.

In **Settings → Pages → Custom domain**, confirm it shows `chimpdandy.com` (the CNAME file sets this). Leave the **Enforce HTTPS** box for step 3.

### 2. Point the domain at GitHub (Porkbun → Domain Management → chimpdandy.com → DNS)

1. Delete Porkbun's default parking records: the **ALIAS** record on the root and the **CNAME** for `*`. They point at `pixie.porkbun.com` / `uixie.porkbun.com`.
   Do **not** delete any **MX** or email **TXT** records. Those carry the email forwarding.
2. Add these records. Leave the host blank for the root domain.

| Type | Host | Answer |
|---|---|---|
| A | *(blank)* | 185.199.108.153 |
| A | *(blank)* | 185.199.109.153 |
| A | *(blank)* | 185.199.110.153 |
| A | *(blank)* | 185.199.111.153 |
| AAAA | *(blank)* | 2606:50c0:8000::153 |
| AAAA | *(blank)* | 2606:50c0:8001::153 |
| AAAA | *(blank)* | 2606:50c0:8002::153 |
| AAAA | *(blank)* | 2606:50c0:8003::153 |
| CNAME | www | treyturner32.github.io |

With both the root and `www` set up, `www.chimpdandy.com` redirects to `chimpdandy.com` automatically.

### 3. Turn on HTTPS

DNS usually takes from a few minutes up to an hour to update. When **Settings → Pages** shows the DNS check as successful, tick **Enforce HTTPS**. The certificate can take up to about an hour to issue.

### 4. Protect the domain (recommended)

GitHub → your profile picture → **Settings → Pages → Add a domain** → `chimpdandy.com`. GitHub shows a TXT record. Add it in Porkbun DNS and click **Verify**. This stops anyone else's GitHub repo from claiming your domain.

### 5. Email forwarding (Porkbun → Domain Management → chimpdandy.com → Email)

Create two forwards:

- `contact` → `treyxturner@gmail.com`
- `trey` → `treyxturner@gmail.com`

Porkbun adds the MX and SPF records it needs. Send a test from a different email account.

Later, to switch to Zoho Mail's free plan: remove these forwards, then follow Zoho's setup to replace the MX/SPF records with Zoho's. The website records above stay as they are.

### 6. Visitor stats (GoatCounter, free, no cookies)

Sign up at goatcounter.com and choose the site code **`chimpdandy`**. The page already reports to `chimpdandy.goatcounter.com`. If that code is taken, pick another and change the `data-goatcounter` URL near the bottom of `index.html` to match.

## Updating the site

Edit, then:

```sh
git add -A && git commit -m "Update" && git push
```

GitHub Pages republishes within a minute or two.
