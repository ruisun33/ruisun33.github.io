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

Each project currently has its own placeholder in assets/img/research/:
trace.svg, primal-dual.svg, add-on.svg, and bandits.svg.
To replace one with a PNG, JPG, SVG, or GIF, put the new file in that folder and
change the image field in the corresponding _research/*.md file. Update image_alt too.
The current placeholders use a 3:1 ratio. Adjust width/height in the visual include
if the replacement images use a different ratio.

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

The two blog articles currently have `published: false` while being revised.
Remove that setting when each article is ready to appear on the site.
