# Shashaank Kumar Sinha: portfolio + Studio dashboard

A static portfolio website with a built-in editor ("Studio"). Everything you can see on the site is stored in
one file, `data/site-data.js`, and the Studio edits that file for you. No build step.

## What is in this folder

```
index.html              the public site
css/site.css            site styles (Tailwind utility classes load from their CDN in index.html)
css/intro.css           sunset intro animation + the scene used on the Studio login page
js/site.js              draws every section from data/site-data.js, intro, certificate viewer
js/chat.js              visitor chat box (works offline; upgrades to Claude if the function is set up)
data/site-data.js       ALL your content: profile, projects, internships, certificates, badges, chat answers
assets/                 pictures, certificate images and PDFs, offer letters (assets/uploads = files added in Studio)
admin/                  Studio: login page + dashboard (index.html, admin.css, admin.js)
netlify/functions/      optional server pieces: visitor counter, visitor map data, Claude assistant, Claude chat
netlify.toml, package.json   settings for Netlify
.nojekyll               tells GitHub Pages to serve the files as they are
```

## 1. Put it on GitHub

1. Create a new repository (for example `portfolio`).
2. Upload everything from this folder (drag the files in on github.com, or `git push`).
3. Host it:
   - **Netlify (recommended):** "Add new site", "Import from Git", pick the repository, leave the defaults. This also turns on the visitor map and the Claude assistant.
   - **GitHub Pages:** Settings, Pages, deploy from the `main` branch, root folder. The site and the Studio editor work; the visitor map and Claude assistant do not (GitHub Pages cannot run functions).

## 2. Sign in to Studio

Open `https://your-site/admin/` (or the "Studio login" link in the site footer).

- User name: `Shashaank Kumar Sinha`
- Password: the one you gave me. It is stored only as a salted hash in `data/site-data.js`, never as plain text.
- Change it in Studio, Settings, Studio login, then press Publish.

Important: the site is static, so the sign-in screen only hides the editor from casual visitors. What actually
protects your website is that **only your GitHub token can publish changes**. Never share that token.

## 3. Let Studio publish for you (one time)

1. On GitHub: Settings, Developer settings, Personal access tokens, **Fine-grained tokens**, Generate new token.
2. Repository access: only your portfolio repository. Permission: **Contents: Read and write**.
3. In Studio, Settings, "Publish to GitHub": enter your GitHub user name, repository name, branch (`main`) and the token. Press "Test connection".
4. From now on: edit anything, then press **Publish**. Studio makes one commit with your changes and uploaded files, and your host updates the site in a minute or two.

The token is kept only in your browser (localStorage). It is never written into the site files.

Until you publish, your edits live as a **draft in this browser**: the public site shows an "Unpublished draft preview" badge on your own device only.

## 4. Visitor map and Claude assistant (Netlify)

In Netlify: Site configuration, Environment variables:

| Name | Value |
|------|-------|
| `ADMIN_TOKEN` | a long random secret you invent (paste the same one in Studio, Settings, Admin token) |
| `ANTHROPIC_API_KEY` | your Anthropic API key (console.anthropic.com) |

- **Visitors page:** the site counts one visit per browser session, keeping only country and city (no IP addresses, no cookies; it respects "Do Not Track"). Studio shows totals, countries, cities and a map. Data is stored in Netlify Blobs.
- **Ask Claude:** an assistant that can add, edit, move and delete content in your draft (projects, internships, certificates, badges, text) and help with writing. It never publishes by itself; you review and press Publish. Each question uses your API credit.
- **Visitor chat box on the site:** answers from your data by itself. If `ANTHROPIC_API_KEY` is set it uses Claude for open questions (limited to 12 questions per minute per visitor).

## 5. Common edits

| I want to... | Where |
|---|---|
| Change photo, name, about, contact | Studio, Profile |
| Add or reorder projects / internships | Studio, Projects / Internships |
| Add a certificate (image or PDF + verify link) | Studio, Certificates, Add certificate |
| Move a certificate between "Certificates" and "Additional certificates" | the layers icon on its row, or edit it and change Section |
| Add a Credly badge | Studio, Digital badges, paste the embed code |
| Change the pictures on the intro boxes | Studio, Settings, Site intro |
| Edit by hand | `data/site-data.js` (plain JSON inside `window.SITE_DATA = {...};`) |

## 6. Things to know

- Tailwind CSS is loaded from `cdn.tailwindcss.com`; fonts from Google Fonts; the Credly badge script from Credly; the visitor map uses Leaflet and map tiles from the internet. Everything else is in this folder.
- Certificate pages that block embedding (some issuers do) still open with the "Open in new tab" / "Verify online" button. Using your own image avoids this.
- LinkedIn posts only embed if they are public.
- Files you remove in Studio are not deleted from `assets/uploads` in the repository; delete them on GitHub if you want to.
- Run it locally with any static server, for example `python3 -m http.server 8000`, then open `http://localhost:8000/`.
