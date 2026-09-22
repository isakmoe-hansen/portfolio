# Isak Moe-Hansen — portfolio site

One self-contained file. No build step, no dependencies, no external requests — the
fonts are embedded in it. Open `index.html` in a browser and it works.

```
index.html                       the whole site
assets/portrait.jpg              photo, used on the CV page
assets/figures/                  the project plots, loaded only on project pages
assets/og-card.png               the 1200 × 630 image shown when the link is shared
```

The site has five sections, all inside that one file:

| Address            | Page                                              |
|--------------------|---------------------------------------------------|
| `#/`               | About and contact, the front page (also `#/about`) |
| `#/projects`       | The intro and the five-project register            |
| `#/p/<name>`       | One project, e.g. `#/p/material-balance`           |
| `#/experience`     | Summer jobs                                        |
| `#/cv`             | Full CV, including the grade chart                 |

Each of those is a real, linkable address. You can send someone
`.../#/p/drilling-advisor` and they land on that project.

---

## Editing your text

### Option A — click and type in the browser

1. Open the site and add `?edit` to the address: `index.html?edit`
   (or press **Ctrl/Cmd + Shift + E** at any time).
2. Every editable piece of text gets a dashed outline. Click one and type.
3. Press **Download**.
4. Move the downloaded `index.html` into this folder, replacing the old one.

Your words change; layout, links and styling are untouched.

### Option B — edit the text directly

Open `index.html` in any text editor and find this, a little over halfway down:

```js
/* CONTENT:START */
const CONTENT = { ... };
/* CONTENT:END */
```

Everything between those two markers is your content, and nothing else in the file is.
It is ordinary JSON: keep the quotes and commas as they are and you cannot break anything
structural. Both editing methods write to the same block, so you can mix them freely.

### Things worth knowing

- **The five projects** live in `projects`. Each one has a `figure`, a `unit` and a
  `figNote` — that is the number in the left-hand column of the front page, the thing the
  project is named by. Pick the number that is the point of the project, not the biggest
  one.
- **`slug`** is what appears in the address bar. Changing it changes the project's URL.
- **Project body text** is a list of `blocks`. The `t` field says what kind each is:
  `h2` (a heading), `p` (a paragraph), `result` (the callout with the black rule down the
  left), `stats` (a row of figures), `eq` (the grey formula box), `table`. To add a
  paragraph, copy an existing `{ "t": "p", "v": "..." }` and change the text.
- **A table row** with `"pick": true` is shaded and marked — that is the option that was
  chosen.
- **The grade chart draws itself** from `cv.chart.semesters`. Add a semester like this and
  the line, the dots, the cumulative average and the data table all follow:
  ```json
  { "term": "Autumn", "year": "2026", "grades": [4, 5, 4, 3] }
  ```
  Grades are numbers: A=5, B=4, C=3, D=2, E=1. `shadeFrom` is the index of the first
  semester shaded as specialisation courses (0 is the first semester).

---

## Putting it online

The site is plain static files, so anything that serves files will host it.

### GitHub Pages (free, gives you `isakmoe-hansen.github.io`)

Create the repository on GitHub first, named exactly `isakmoe-hansen.github.io`. Then:

```bash
cd "/Users/isakmh/Documents/Porfolio website"
git init
git add index.html assets README.md
git commit -m "Portfolio site"
git branch -M main
git remote add origin https://github.com/isakmoe-hansen/isakmoe-hansen.github.io.git
git push -u origin main
```

Then Settings → Pages → Source: `main` / root. It goes live at
`https://isakmoe-hansen.github.io` within a minute or two.

To publish a change later:

```bash
git add -A && git commit -m "Update text" && git push
```

### Previewing locally

```bash
python3 -m http.server 4173
```

Then open `http://localhost:4173`. Double-clicking `index.html` also works.

---

## Design notes

- **Restrained academic.** Paper `#FBFBF8`, ink `#16181C`, one accent (`#2B4A6F`) used on
  links and nothing else. Set in Source Serif 4, with IBM Plex Mono carrying every label,
  caption and piece of metadata. Both are open-licensed (SIL OFL) and embedded in the
  file, so the site makes no network requests at all.
- **The figure register** is the signature device: each project is identified on the front
  page by the number that is its point, not by a thumbnail. It is what replaces the
  equations in the site this one is modelled on.
- **Light mode only**, deliberately. `color-scheme: light` is declared so browsers do not
  auto-invert it.
- **Printing** drops the navigation, the download bars and the buttons. The CV page prints
  as a CV.
- **Accessibility**: skip link, visible focus rings, sequential headings, a text
  alternative for the grade chart (the "Show the figures as a table" toggle), and all text
  clearing WCAG AA on the paper background.
