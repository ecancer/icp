# Institute of Cancer Policy — Hugo Website

## Tech stack

- **Static site generator**: Hugo (v0.120+)
- **Hosting**: Netlify (recommended) or GitHub Pages
- **Forms**: Netlify Forms (newsletter signup)
- **Analytics**: Plausible Analytics (privacy-respecting, no cookies)
- **Fonts**: Google Fonts — Playfair Display + DM Sans
- **No framework, no build tool, no JavaScript dependencies**

---

## Quick start

### 1. Install Hugo

```bash
# macOS (Homebrew)
brew install hugo

# Windows (Chocolatey)
choco install hugo-extended

# Linux
snap install hugo
```

Verify: `hugo version` (needs v0.120 or later)

### 2. Run locally

```bash
cd icp-site
hugo server -D
```

Open http://localhost:1313 in your browser. Pages reload automatically as you edit.

### 3. Build for production

```bash
hugo --minify
```

Output goes to the `public/` folder. Deploy this folder to any static host.

---

## Project structure

```
icp-site/
├── hugo.toml              # Site config, menu, global params
├── content/               # All editable content (Markdown)
│   ├── _index.md          # Homepage (no body — layout handles it)
│   ├── research/          # Research papers
│   ├── policy/            # Policy reports
│   ├── events/            # Events
│   ├── team/              # Team profiles
│   └── about/             # About, contact, privacy pages
├── layouts/               # Hugo templates (HTML)
│   ├── index.html         # Homepage layout
│   ├── _default/          # Fallback list + single templates
│   ├── events/            # Events-specific layouts
│   ├── team/              # Team-specific layouts
│   ├── about/             # About page layout
│   └── partials/          # Reusable components
│       ├── head.html
│       ├── nav.html
│       ├── footer.html
│       ├── cta-band.html
│       └── illustrations.html   # All SVG illustrations
├── static/
│   ├── css/main.css       # Full design system
│   ├── images/            # Drop real photography here
│   └── js/                # Optional JS (none required)
└── archetypes/            # Templates for `hugo new` command
```

---

## Adding content

### New research paper

```bash
hugo new research/my-paper-title.md
```

Edit the generated file in `content/research/`. Key front matter fields:

| Field | Description |
|-------|-------------|
| `title` | Full paper title |
| `date_display` | Human-readable date, e.g. "March 2026" |
| `journal` | Journal name |
| `topic` | One of: Global access, Health economics, Prevention policy, Cancer systems |
| `illustration` | One of: card-lmic, card-econ, card-policy |
| `excerpt` | One-sentence description for cards |
| `authors` | YAML list of author strings |
| `doi` | DOI without https://doi.org/ prefix |
| `pdf_url` | Path to PDF in /static/ |

### New event

```bash
hugo new events/my-event.md
```

Key fields: `day`, `month`, `year`, `event_type`, `badge_style`, `location`, `time`, `register_url`

### New team member

```bash
hugo new team/firstname-lastname.md
```

Key fields: `role`, `initials`, `avatar_colour` (amber/blue/teal/gray), `focus`

---

## Replacing illustrations with photography

All SVG illustrations are in `layouts/partials/illustrations.html`.
Each is guarded by an `{{ if eq . "name" }}` block.

To use a real photo instead, in the relevant layout template replace:

```html
{{ partial "illustrations.html" "hero" }}
```

with:

```html
<img src="/images/hero-field.jpg"
     alt="A health worker in rural Uganda"
     style="width:100%;height:100%;object-fit:cover;" />
```

### Image specs

| Zone | File | Min size | Ratio |
|------|------|----------|-------|
| Hero panel | `hero-field.jpg` | 680×760px | Portrait |
| Director portrait | `director-portrait.jpg` | 440×480px | Portrait |
| LMIC card | `card-lmic.jpg` | 600×200px | 3:1 landscape |
| Economics card | `card-economics.jpg` | 600×200px | 3:1 landscape |
| Policy card | `card-policy.jpg` | 600×200px | 3:1 landscape |
| Field feature | `field-feature.jpg` | 900×520px | 16:9 landscape |
| Team headshots | `team-[name].jpg` | 200×200px | Square (circle-cropped) |

---

## Deploying to Netlify

1. Push this project to a GitHub or GitLab repository
2. Log in to Netlify → New site from Git → select your repo
3. Build command: `hugo --minify`
4. Publish directory: `public`
5. Set environment variable: `HUGO_VERSION = 0.120.0`

Netlify will deploy automatically on every push to `main`.

### Newsletter form

The newsletter form uses Netlify Forms. It works automatically on Netlify with no configuration — submissions appear in the Netlify dashboard under Forms.

To use a third-party email provider instead (Mailchimp, Buttondown, ConvertKit), replace the `<form>` action in `layouts/partials/cta-band.html`.

---

## Custom domain

In Netlify: Site settings → Domain management → Add custom domain.
Add a CNAME record pointing your domain to your Netlify site URL.

---

## Dark mode

Dark mode is fully implemented via `@media (prefers-color-scheme: dark)` in `static/css/main.css`. It follows the user's OS setting automatically.

To add a manual toggle button, add this to `layouts/partials/nav.html`:

```html
<button onclick="toggleDark()" id="dark-toggle">◐</button>
```

And this script to `layouts/partials/footer.html`:

```html
<script>
function toggleDark() {
  document.documentElement.classList.toggle('dark');
  localStorage.setItem('theme', document.documentElement.classList.contains('dark') ? 'dark' : 'light');
}
if (localStorage.getItem('theme') === 'dark') document.documentElement.classList.add('dark');
</script>
```

Then duplicate the `@media (prefers-color-scheme: dark)` block in `main.css` as `:root.dark { ... }` to support the class-based toggle alongside the media query.

---

## Hugo version compatibility

This project requires Hugo v0.120 or later (extended edition recommended). The extended edition supports SCSS if you later wish to split the CSS into partials — not required now but useful as the site grows.
