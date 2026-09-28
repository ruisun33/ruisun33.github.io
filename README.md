# Rui Sun’s personal website

A Jekyll site using the Chirpy theme. Local files override the matching files in
that theme. The theme itself is installed through the Gemfile; do not edit vendor/.

## Everyday editing

| File | Purpose |
| --- | --- |
| index.md | Homepage headline, contact icons, bio, and news include |
| _tabs/research.md | Projects page introduction |
| _research/*.md | Each project’s title, order, keywords, visual, links, and description |
| _data/news.yml | News entries, newest first |
| _posts/*.md | Blog posts, with title, date, tags, and visibility in front matter |
| _tabs/blog.md | Blog page title and introduction |
| assets/404.html | Custom page-not-found message and navigation |
| _data/share.yml | Sharing settings; currently Copy link only |
| _config.yml | Site metadata, avatar, social profiles, theme and build settings |
| assets/css/jekyll-theme-chirpy.scss | Custom styles, following the theme import |

Browser titles use `title` followed by ` | Rui Sun`; the homepage uses `Rui Sun`.
For a long post title, add `browser_title: "Shorter title"` to its front matter.
This changes the browser tab title while preserving the full visible heading.
The title logic lives in `_includes/head.html`, which overrides the theme’s head.

## How a page is assembled

index.md -> _layouts/home.html -> the theme’s default layout.
The homepage calls _includes/news.html, which reads _data/news.yml.
Projects use the theme’s page layout. _includes/research.html reads the project
collection from _research/ and displays image-and-text rows. These are separate
from blog posts and do not generate individual project URLs.
The blog tab calls _layouts/blog.html, which lists visible posts.
Posts use the theme’s post layout. _includes/footer.html overrides the shared footer.
_includes/metadata-hook.html adds local preview CSS cache handling and theme controls.

## Project visuals

Editable draw.io sources live in diagrams/research/. Open the .drawio files in
https://app.diagrams.net/ using File > Open From > Device. Keep these sources in
Git; the diagrams/ folder is excluded from the generated website.
Export finished diagrams as SVG to assets/img/research/.

TRACE currently uses trace-wide.svg, with source diagrams/research/trace-wide.drawio.
The original square layout remains in trace.svg and diagrams/research/trace.drawio.
The primal-dual entry uses
primal-dual.svg, with editable source in diagrams/research/primal-dual.drawio.
Add-on discounts uses add-on.svg with source diagrams/research/add-on.drawio.
Bandit matching uses bandits.svg with source diagrams/research/bandits.drawio.
All four research entries now have editable diagrams.
To replace an image with a PNG, JPG, SVG, or GIF, put the new file in assets/img/research/
and update image, image_alt, image_width, and image_height in its _research entry.
All research diagrams use the same displayed width, controlled by
--research-visual-width in assets/css/jekyll-theme-chirpy.scss (currently 100%).
Height follows each image's aspect ratio; clicking opens the full-size SVG.
The original TRACE layout is retained as trace.svg and trace.drawio.

## News

Each entry has date (YYYY-MM), label (display date), category, title, text, and url.
Only title is linked; text is an optional suffix, including its desired spacing.
Categories: Preprint, Publication, Blog, Release, Life update.
Category emoji are mapped in _includes/news.html. The newest five entries display.

## Local preview

From the repository folder:

```sh
PATH=/opt/homebrew/opt/ruby@3.3/bin:$PATH BUNDLE_PATH=vendor/bundle bundle install
PATH=/opt/homebrew/opt/ruby@3.3/bin:$PATH BUNDLE_PATH=vendor/bundle bundle exec jekyll serve --livereload --force_polling --host 127.0.0.1 --port 4000
```

Open http://127.0.0.1:4000/. Saving content rebuilds the site. Changes to
_config.yml require stopping and restarting the server. Ctrl+C stops it.

## Cleanup candidates and things to keep

- The unused publication data, publication include, and publication list styles
  have been removed. Research entries now live in _research/.
- DCM and Ad Arena are stored in _drafts/ and excluded from normal builds.
  To preview drafts, add --drafts to the Jekyll serve command. Move a finished
  draft to _posts/ with a YYYY-MM-DD-title.md filename to publish it.
- The duplicate TRACE blog post was removed; its research entry is _research/trace.md.
- _includes/update-list.html and _includes/trending-tags.html are intentionally
  empty overrides. Keep them to suppress the theme’s default sidebar widgets.
- _data/contact.yml is used by the theme’s sidebar; the homepage contact links
  are configured separately in index.md. Avoid removing it without checking the
  sidebar, including mobile behavior.
- categories.html and tags.html support blog archives. These are separate from
  any future project keyword labels or filters.
- _site/, vendor/, .jekyll-cache/, and Gemfile.lock are generated or local files
  ignored by this repository. Do not edit _site/: rebuilds overwrite it.
- .github/workflows/ controls deployment. The current workflow deploys main/master,
  not dev. Local changes do not publish the live website.
- LICENSE preserves the starter’s license notice. Keep it.

## Other folders

_data/ contains structured content and settings; _includes/ contains reusable
fragments; _layouts/ assembles pages; _plugins/ contains a post modification-date
hook; assets/ holds images and styling; tools/ contains preview and validation
scripts. .devcontainer/ and .vscode/ are optional development environment settings.

## Project metadata

Keep `order` to choose the display sequence (lower numbers first). Optional `code`
and `huggingface` fields accept a URL or `coming-soon`; omit them to hide the link.
Replace `coming-soon` with the real URL when a resource is released.
Titles and keywords appear above the image and summary.

Research content lives in `_research/`, with the page at `/research/`.
The root `projects.html` preserves old `/projects/` links and their section anchors.
Both navigation pages now use Markdown files under `_tabs/`.

## Data and inherited layouts

`_data/news.yml` stores short structured news entries rather than standalone pages.
Jekyll exposes these as `site.data.news`; `_includes/news.html` renders the list.
`_research/` is a collection because each paper has its own metadata and Markdown body.

Only `home.html` and `blog.html` need local layout overrides. The Chirpy gem supplies
`default`, `page`, `post`, `compress`, and archive layouts. Local files with matching
names override the theme. The Research tab uses the inherited `page` layout and the
local research include, so it does not need a separate research layout.

The two reward learning articles are also stored in `_drafts/` while being revised.
To publish one, move it to `_posts/` with a `YYYY-MM-DD-title.md` filename.
Their original dates are preserved in the front matter.

Research sections show the full description followed by the diagram.
The order is controlled by _includes/research.html.

The research page uses these wide diagrams: trace-wide.svg,
primal-dual-wide.svg, add-on-wide.svg, and bandits-wide.svg. Their editable
sources have matching names in diagrams/research/. Earlier versions are retained.

Latest canvas sizes: TRACE and primal-dual 1760 × 625; add-on 1670 × 625;
bandits 1730 × 625. Primal-dual-wide now contains the selected balanced layout.

Unpublished writing in `_drafts/` is local-only and ignored by Git. Move an article
to `_posts/` when it is ready to be tracked and published.
