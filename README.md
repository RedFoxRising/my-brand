# mauroguerrero.com

Personal site of Andres Mauricio Guerrero Avila — economics student at Universidad de Los Andes. Portfolio and writing on data analysis, econometric modeling, and business strategy.

**Live:** [mauroguerrero.com](https://mauroguerrero.com/)

## Built with

- **[Hugo](https://gohugo.io/)** 0.164.0 extended — the version pinned in CI; using the same one locally avoids surprises
- **[PaperMod](https://github.com/adityatelange/hugo-PaperMod)** — included as a git submodule under `themes/`
- **Bilingual EN/ES** — every page exists in both languages, paired by `translationKey`. Content lives in `content/en/` and `content/es/`, served under `/en/` and `/es/`
- **GitHub Actions → GitHub Pages** — every push to `main` builds and deploys to the apex domain

Customizations live outside the theme so it stays updatable: `layouts/` holds partial overrides, `assets/css/extended/custom.css` holds the palette.

## Running locally

Clone with the theme submodule:

```bash
git clone --recurse-submodules https://github.com/RedFoxRising/my-brand.git
```

If you already cloned without it:

```bash
git submodule update --init --recursive
```

Then start the dev server:

```bash
hugo server
```

The site is at `http://localhost:1313`. Hugo rebuilds on save.

Drafts are hidden unless you ask for them:

```bash
hugo server -D
```

To check what will actually publish, without guessing from the browser:

```bash
hugo list published
```

## Structure

```
content/en/, content/es/   Content, mirrored across languages
layouts/                   Overrides of PaperMod partials
assets/css/extended/       Custom palette and styles
static/                    Images, CV PDFs, favicons
hugo.toml                  Config: languages, menus, params
.github/workflows/         Build and deploy
```

## Deploying

Push to `main`. The workflow in `.github/workflows/deploy.yml` builds with Hugo and publishes to GitHub Pages — no manual step. Watch progress in the Actions tab.

## Credits

Started as a fork of [student-branding-starter](https://github.com/lukelittle/student-branding-starter) by Lucas Little — an MIT-licensed Hugo template for students. The scaffolding, the GitHub Actions workflow, and the bilingual setup came from there; the content, palette, and layout overrides are mine.

Theme: [PaperMod](https://github.com/adityatelange/hugo-PaperMod) by Aditya Telange.

## License

Template and theme are MIT (see [LICENSE](LICENSE)). Content — posts, project writeups, CV — © Andres Mauricio Guerrero Avila, all rights reserved.
