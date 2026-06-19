# Front Range WELS

Website for the group of Wisconsin Evangelical Lutheran Synod (WELS) churches
along Colorado's Front Range, including the annual Front Range softball
tournament history.

It is a [Jekyll](https://jekyllrb.com/) site hosted on **GitHub Pages** and
served at **https://www.frontrangewels.com**. GitHub builds and deploys it
automatically on every push to `main` — no manual build step is required to
publish.

## Run it locally at http://localhost:8767/

### Prerequisites
- Ruby and Bundler (`gem install bundler` if needed)

### First time
```bash
bundle install
```

### Launch the dev server
```bash
bundle exec jekyll serve --port 8767
```
Then open **http://localhost:8767/**. The server rebuilds automatically as you
edit files; refresh the browser to see changes.

Press `Ctrl+C` to stop it. If you started it detached (`--detach`) or the port
is stuck, free it with:
```bash
lsof -ti tcp:8767 | xargs kill
```

> Note: the `Gemfile` pins `webrick`, `bigdecimal`, and `base64` because recent
> Ruby versions removed them from the standard library — they're only needed
> for local preview. GitHub Pages builds with its own bundled Jekyll.

## Project structure

```
_config.yml          Site config (title, URL, plugins, sitemap rules)
_layouts/default.html Page shell: <head> (SEO, structured data, analytics), header, footer
_includes/
  header.html        Site title + navigation (shared by every page)
  footer.html        Footer columns + copyright (shared by every page)
assets/css/styles.css All site styles
index.html           Home page (hero, about, church-locator map)
softball.html        Softball tournament page (year selector + loader)
reformation.html     Event pages (Coming Soon)
pickleball.html
bandfest.html
404.html             Not-found page
softball/
  <year>.html        Per-year tournament content fragments (loaded via fetch())
  images/            Team photos
CNAME                Custom domain for GitHub Pages
robots.txt           Points crawlers to the sitemap
```

Each page is a thin file with YAML front matter (`title`, `description`) that
uses the shared layout, so the header, footer, and `<head>` live in one place.

## Editing common things

- **Header or footer / navigation:** edit `_includes/header.html` or
  `_includes/footer.html` — changes apply to every page.
- **Styles:** edit `assets/css/styles.css`.
- **A page's title/description (SEO):** edit the front matter at the top of that
  page's `.html` file.
- **Add or update a softball year:** create or edit
  `softball/<year>.html` (a plain HTML fragment — no front matter), and make
  sure the year has an `<option>` in the dropdown in `softball.html`. The page
  fetches the fragment when the year is selected.

## SEO & metadata

- `jekyll-seo-tag` generates titles, meta descriptions, and OpenGraph tags from
  each page's front matter plus `_config.yml`.
- `jekyll-sitemap` generates `/sitemap.xml` (the `softball/<year>.html`
  fragments are excluded via `_config.yml`, since they are partials, not pages).
- Organization structured data (JSON-LD) and the Cloudflare Web Analytics beacon
  live in `_layouts/default.html`.

## Deployment

Push to `main`. GitHub Pages rebuilds the site with Jekyll and serves it at the
domain in `CNAME` (`www.frontrangewels.com`). The plugins used
(`jekyll-sitemap`, `jekyll-seo-tag`) are both supported by GitHub Pages.
