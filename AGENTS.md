# AGENTS.md

Context for any AI agent (or person) working in this repo.

## What this project is

A curated gallery of 100 public-domain artworks proposed as the cover of the book
**"Tres orillas de la libertad. Derechos civiles y garantías constitucionales en Venezuela,
Estados Unidos y España"**: a comparative constitutional-law monograph by a Venezuelan judge
who is a candidate for magistrate of the TSJ (Tribunal Supremo de Justicia).

Book thesis (drives every selection and tone decision):

- **United States** = negative liberty (a shield against the State).
- **Europe / Spain** = positive liberty (dignity and solidarity).
- **Venezuela** = nominal constitutionalism (hyper-guaranteeing constitution on paper, eroded in practice).

The site lets the author browse the works, mark favorites ("Elegir"), and copy the selection.

- Live site: https://ijorgesilva.github.io/tres-orillas-de-la-libertad/
- Repo: https://github.com/ijorgesilva/tres-orillas-de-la-libertad (public, GitHub Pages from `main`, root)

## Layout

| Path | Purpose |
|---|---|
| `index.html` | The site. Single self-contained file (CSS + JS inline). Served by Pages. |
| `Cien obras para la portada de Tres orillas de la libertad.html` | Original copy of the same page, kept as the author received it. Keep in sync with `index.html` or retire it. |
| `images/NNN.jpg` | One thumbnail (~640px) per work; `NNN` is the zero-padded work number (`001`..`100`). |
| `images/sources.json` | Wikimedia Commons file title each image came from (first-pass search; not every entry is hand-verified). |
| `FICHAS.md`, `fichas.html` | Per-work research sheets (qualification, verified story, cover suitability, risks), when present. |

## Data model

All works live in the JS array `D` inside `index.html`. Each row is:

`[type, group, title, original title, artist, year, museum, catalog comment, search query, optional warning]`

- `type`: `"a"` abstract (50) or `"h"` with history (50).
- The **work number is the 1-based position in `D`**. It is also the image filename and the
  key used in saved picks. Never reorder or insert rows in the middle; append or edit in place,
  otherwise images and users' saved selections point at the wrong works.
- Groups and colors: `COL` and `NOTE` maps in the same file.

## Features to preserve

- Filters: type (Todas / Abstractas / Con historia), group chips, free-text search (accent-insensitive).
- **Lista / Cuadrícula** view toggle (`#view`), persisted in `localStorage["portada-view"]`.
- Picks persisted in `localStorage["portada-picks"]`; "Ver solo mi selección" and "Copiar mi selección".
- Missing image falls back to the text "Imagen no disponible" (JS `error` handler on `#main`).
- Dark mode via CSS variables; layout must work at phone width.

## Image sourcing rules

- Images come from Wikimedia Commons via its API (`generator=search`, `iiurlwidth=640`), downloaded
  locally so the site never hotlinks.
- Python's bundled urllib fails SSL verification on this machine; use `curl` for fetching.
- Wikimedia asks for a descriptive User-Agent; send one and pace requests (~0.7s apart).
- **Verify every match visually or by file title.** A surname-only check is not enough: work 83
  ("Rafael") once matched a photo of Rafael Correa. Wrong image is worse than a placeholder: delete
  the file and let the fallback show.
- Known gaps (placeholder shown): 2, 27, 44, 46. Known close-but-not-exact: 5, 29, 75, 82.

## Public-domain criteria (do not loosen)

- Artists who died **before 1956** (60 years in Venezuela, 70 in most of the world).
- Exceptions are flagged in the catalog's warning field (e.g. Mondrian's *Broadway Boogie Woogie* in the US,
  Orozco's murals in Mexico).
- Classic Venezuelan abstraction (Soto, Cruz-Diez, Otero, Gego) is still protected and intentionally excluded.
- A faithful photo of a public-domain painting adds no new rights in Venezuela or the EU, but
  each Commons file license must be checked before print. This is practical guidance, not legal advice.

## Audience and tone

- Spanish (neutral), sober and institutional: the author is a judge seeking a high-court seat.
- Avoid em-dashes in UI copy.
- Cover judgments weigh: title legibility (free space, contrast), crop options, graphic violence,
  political charge (especially around Venezuela today), and public-domain doubts.

## Roadmap (requested by the author, in order)

1. **Fichas for the 100 works** (Spanish, numbered like the catalog): why it qualifies and which
   shore/idea of the thesis it reinforces; the story behind it, verified by web search; cover suitability
   (title legibility, crop, tone for a TSJ candidate); risks (violence, political charge, public-domain
   doubts). Correct any wrong year or museum. Deliver as `FICHAS.md` + `fichas.html`.
2. **50 new works**, none repeating the catalog. Criteria: public domain (artist died before 1956),
   verified by web search. Priority order: Venezuela and Latin America, then Spain, then the rest.
   Look for stories tied to constitutions, rights, justice, judges, abuse of power, or shores and rivers,
   plus sober abstracts suited to an academic book. Use the same fields as the catalog: title, original
   title, artist, year, museum, story, search term. Append them to `D` as works 101-150 (never reorder),
   add their images, then republish the same artifact at
   https://claude.ai/artifact/BzW1Lz9GimEvt6Rnnd5afv (update in place via the Artifact tool with that `url`;
   read it first). Update the page copy that says "cien" / "100" accordingly, and keep `index.html` in sync.

## Working here

- No build step. Preview with `python3 -m http.server 8765` in the repo root.
- Deploy = push to `main`; Pages rebuilds in about a minute.
- Commit types: feat, fix, refactor, docs, test, chore, perf, ci. No attribution trailers.
- Git is the only source of truth for the images; keep `images/` under ~30 MB.
