# CLAUDE.md

## Project overview

**Le Plus Grand Nombre** — site vitrine (Hugo) pour leplusgrandnombre.fr. Le positionnement : expertise data appliquée à toute la chaîne marketing. Les 5 projets live ne sont pas des produits — ce sont des preuves de concept qui démontrent la capacité à intervenir à chaque étape :

1. **Comprendre son marché** → Geomarketing
2. **Segmenter & cibler** → *(pas de démo encore)*
3. **Trouver le bon angle** → Signal
4. **Créer du contenu qui me ressemble** → Coaching
5. **Activer & distribuer** → *(pas de démo encore)*
6. **Gagner du temps au quotidien** → Artisans
7. **Le KPI utile, votre vision en plus** → The Shift Project
8. **Fidéliser & engager** → *(pas de démo encore)*

Posture : pas un expert data qui vend de la technique — quelqu'un qui comprend le contexte, les envies et les challenges du client, et qui utilise la data/IA comme moyen pour l'aider à aller là où il veut aller. Toujours partir du problème client, jamais de la techno.

## Tech stack

- **Hugo** (extended version required) with a custom theme `lpgn` in `themes/lpgn/`
- **Formspree** for contact form handling
- **GitHub Pages** for hosting, deployed via GitHub Actions on push to `main`
- Domain: `leplusgrandnombre.fr`, DNS via AWS Route 53

## Project structure

```
config/_default/     Hugo config (hugo.toml, params.toml, menus.toml, markup.toml)
content/             Site pages (blog/, about, contact, applications)
layouts/shortcodes/  Custom shortcodes (contact-form.html)
themes/lpgn/         Custom theme (layouts, partials, CSS)
static/              Static assets
data/                Hugo data files
i18n/                UI translations
.github/workflows/   CI/CD (hugo.yml)
```

## Development

```bash
hugo server          # Local dev server at http://localhost:1313
hugo new content/blog/my-post.md   # New blog post
```

Set `draft: false` in frontmatter to publish a post.

## Live projects (subdomains)

Each project has its own subdomain and a README in `projects/`:

| Project | Subdomain | README |
|---------|-----------|--------|
| Artisans | https://artisans.leplusgrandnombre.fr/ | `projects/README_Artisans_ai.md` |
| Coaching | https://coaching.leplusgrandnombre.fr/ | `projects/README_coaching.md` |
| Geomarketing | https://geomarketing.leplusgrandnombre.fr/ | `projects/README_geomarketing.md` |
| Signal (in-culture) | https://signal.leplusgrandnombre.fr/ | `projects/README_in-culture.md` |
| The Shift Project (interactive energy dashboard, Jancovici persona) | https://theshiftproject.leplusgrandnombre.fr/ | `projects/README_interactive_dashboard.md` |

## Key conventions

- The active theme is `lpgn` (set in `hugo.toml`), not the Blowfish submodule referenced in `.gitmodules`
- Site language is French (`languageCode = "fr"`)
- Content is in markdown with Hugo frontmatter
- Custom theme files override defaults via Hugo's lookup order
