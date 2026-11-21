# Codex handoff — Pine Tree Ward Relief Society

Last updated: 2026-11-20

## Project

- Local folder: `D:\repo\pine-tree-ward-rs`
- GitHub repository: `https://github.com/adamulrich/pine-tree-ward-rs`
- Branch: `main`
- Production site: `https://rs.pinetreeward.org`
- Stack: Vite + vanilla HTML/CSS/JavaScript; no runtime Node server.
- The EQ project is separate in `D:\repo\pine-tree-ward`.

## Current site features

- Mobile-first, high-contrast Relief Society one-page site.
- Published Google Sheets CSV feeds for announcements, service opportunities, lesson schedule, recurring schedule, and banner.
- Lesson schedule displays the instructor before the lesson text when the `Instructor` column is populated.
- A `Lesson Summaries` section appears immediately above the lesson schedule. It shows the newest summary first, shows one by default, and expands older summaries with a button.
- Lesson summary data is read from the `Summary` column (also accepts `Lesson Summary`) on the lesson schedule feed. Paragraphs are separated by blank lines and rendered with extra spacing.
- The hero quick links and top navigation include Lesson Summaries. The section uses a note/document SVG icon.
- Bulletin archive is still implemented but hidden on the website. Newsletter archive remains visible.
- PWA manifest, PNG icons, service worker, and install guidance are in `static/`.
- Temple and Family History and Missionary Efforts sections contain the current static goals/content.

## Main files

- `index.html` — page structure, navigation, section markup.
- `src/main.js` — feed URLs, rendering, expandable lists, PWA install behavior.
- `src/data.js` — standard sheet row normalization and optional instructor formatting.
- `src/schedule.js` — recurring schedule parsing/occurrence logic.
- `src/come-follow-me.js` — generated Come, Follow Me data and date selection.
- `styles/styles.css` — plum/rose RS theme and responsive styles.
- `static/manifest.webmanifest`, `static/sw.js`, `static/icons/` — PWA assets.
- `scripts/update-come-follow-me.js` — refreshes generated Come, Follow Me data.
- `.github/workflows/deploy-pages.yml` — GitHub Pages deployment.
- `BulletinArchiver.gs` — Gmail-to-GitHub archive script, including RS newsletter handling.

## Published sheet configuration

The current RS spreadsheet publication ID is:

`2PACX-1vTv57vOlsr8-lYDMxlcC5h30BTIE87EH7kgrbhmbLUViM3bjd8_xCkzoQUO78iyOU6UDvsz94aWtrwB`

Configured gids in `src/main.js`:

- `0` — announcements
- `1654438872` — service opportunities
- `1834157588` — lesson schedule, including `Instructor` and optional `Summary`
- `97794274` — recurring schedule
- `1494821455` — banner

The site reads published CSVs at runtime; spreadsheet edits do not require a build or push. The published sheet must remain publicly readable.

## Local development and verification

```bash
npm install
npm run dev
npm test
npm run build
```

Do not open `index.html` directly; use Vite so modules and remote requests work correctly.

## Deployment

Pushes to `main` trigger the GitHub Pages workflow. GitHub Pages is configured for Actions and the custom domain `rs.pinetreeward.org`; `static/CNAME` contains the domain. Namecheap needs:

```text
CNAME  rs  adamulrich.github.io
```

If a deployment appears not to publish, check the workflow first. A prior Pages artifact lookup failure was resolved by rerunning the workflow; the current icon deployment completed successfully after rerun.

## Important conventions

- Use `apply_patch` for source edits.
- Preserve the existing feed URLs and column names unless explicitly asked to change them.
- Run `npm test` before pushing changes.
- Avoid changing the separate EQ repository unless the user explicitly asks for an EQ change.
