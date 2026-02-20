# leplusgrandnombre.fr

Landing page and blog for Le Plus Grand Nombre, built with [Hugo](https://gohugo.io/) and the [Blowfish](https://blowfish.page/) theme.

## Local development

### Prerequisites

Install Hugo (extended version): https://gohugo.io/installation/

### Run locally

```bash
git clone --recursive https://github.com/YOUR_USERNAME/leplusgrandnombre-website.git
cd leplusgrandnombre-website
hugo server
```

The site will be available at `http://localhost:1313`.

### Create a new blog post

```bash
# French
hugo new content/fr/blog/my-post.md

# English
hugo new content/en/blog/my-post.md
```

Edit the generated markdown file and set `draft: false` when ready to publish.

## Deployment

The site deploys automatically to GitHub Pages via GitHub Actions on every push to `main`.

### First-time setup

1. Push this repo to GitHub
2. Go to **Settings > Pages** in your GitHub repo
3. Under "Build and deployment", select **GitHub Actions** as the source
4. The site will deploy automatically on the next push

### Custom domain setup (Route 53)

In your AWS Route 53 hosted zone for `leplusgrandnombre.fr`, add:

**A records** (root domain):
```
leplusgrandnombre.fr → 185.199.108.153
leplusgrandnombre.fr → 185.199.109.153
leplusgrandnombre.fr → 185.199.110.153
leplusgrandnombre.fr → 185.199.111.153
```

**CNAME record** (www):
```
www.leplusgrandnombre.fr → YOUR_USERNAME.github.io
```

Then enable "Enforce HTTPS" in GitHub Pages settings.

### Contact form (Formspree)

1. Create a free account at [formspree.io](https://formspree.io)
2. Create a new form and copy the form ID
3. In `layouts/shortcodes/contact-form.html`, replace `YOUR_FORM_ID` with your actual form ID

## Project structure

```
config/_default/          # Hugo configuration (bilingual)
content/fr/               # French content
content/en/               # English content
layouts/shortcodes/       # Custom shortcodes (contact form)
i18n/                     # UI translations
static/                   # Static files (CNAME, images)
themes/blowfish/          # Blowfish theme (git submodule)
```

## Tech stack

- **Hugo** — Static site generator
- **Blowfish** — Hugo theme (corporate, bilingual, dark/light mode)
- **Formspree** — Contact form backend
- **GitHub Pages** — Free hosting with HTTPS
- **GitHub Actions** — CI/CD (auto-deploy on push)
