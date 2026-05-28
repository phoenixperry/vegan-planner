# The Lean Vegan

A mobile-first weekly meal planner for a **high-protein, high-calcium vegan cut**.
Every generated day is constrained to **1200–1400 kcal**, **≥107 g protein** (1.6 g/kg
at 67 kg), and pushed toward the **1000 mg calcium** RDA. Two daily flat whites
(480 ml fortified soy milk) are pinned into every day automatically.

No build step, no dependencies, no backend — `index.html` plus two editable data files,
`meals.csv` and `ingredients.csv`. Because the app **fetches** those CSVs at startup it must be
**served over http** (see *Run it*); opening `index.html` straight from the file system won't load them.

## Features
- **Today** — the day's plate (coffee + breakfast + lunch + dinner ± one snack), live macro
  bars vs target, swipe or tap arrows to move between days, per-meal `swap`, and **tap any
  meal to expand its full recipe** (ingredients with quantities + numbered method).
- **Shop** — the whole week's ingredients aggregated by aisle, tickable, copy-to-clipboard.
- **Plan** — load one of **three curated starter weeks**, generate a fresh random week, or
  reroll a single day, from the built-in meal database.

## How the generator works
Each day is solved by **exhaustive search**, not random sampling. It enumerates every
`breakfast × lunch × dinner × (no snack | one of 9 snacks)` combination
(9 × 10 × 10 × 10 = 9 000), keeps only those that land inside the kcal band **and** clear the
protein floor, then prefers combinations whose mains weren't used on the previous day for
variety. If no in-band combination meets the variety constraint it relaxes variety before it
relaxes the nutrition targets — the macros are never sacrificed. Validated over 400 simulated
weeks: 100% of days in-band, 100% ≥107 g protein, ~15–16 distinct mains/week (1 417 of the
9 000 combinations are valid). Three hand-picked **starter weeks** ship ready to load.

Every meal's **kcal / protein / calcium is computed from its ingredients**, so the numbers stay
consistent with what's on the plate.

## Editing the meals (CSV)
The whole database lives in two CSVs you can edit in any spreadsheet or text editor — no HTML:

- **`ingredients.csv`** — `item, kcal, protein_g, calcium_mg, unit, aisle, basis`. Values are
  per **100 g/ml**, unless `basis` is `unit` (then per each/slice — e.g. an apple). `aisle`
  drives the shopping-list grouping.
- **`meals.csv`** — `type, id, name, time, serves, src, ingredients, steps`. `type` is
  `breakfast` / `lunch` / `dinner` / `snack` / `fixed`. `ingredients` is a `Item:qty` list
  separated by `;` (e.g. `Calcium-set firm tofu:150; Spinach:60`). `steps` are separated by `|`.

To **add a meal**, append a row to `meals.csv` with a unique `id`; it joins the generator pool
and `swap` automatically. If it uses a new ingredient, add that ingredient to `ingredients.csv`
first (otherwise the app reports the unknown ingredient). The three curated starter weeks are
defined as `PRESETS` in `index.html` and reference meal ids.

## Run it
Serve the folder (required — the app fetches the CSVs, so `file://` won't work):
```bash
python3 -m http.server 8000   # then visit http://localhost:8000
```

### Deploy on GitHub Pages
Push this repo, then **Settings → Pages → Source: `main` / root**. Your planner is live at
`https://<user>.github.io/<repo>/`. State (current plan, ticked shopping items) persists in
`localStorage` on the device.

## health-tracker.xlsx
A companion logbook: editable targets, a daily log with target-aware conditional formatting
and a trailing 7-day weight average, an adherence dashboard with weight/calorie charts, and a
reference sheet mirroring the app's meal database.

## Health note
This is a genuine calorie deficit — pair it with resistance training to hold muscle. A vegan
plan can't plate everything: supplement **B12** daily, get **iodine** (kelp / iodised salt),
**vitamin D** in winter, and **algal EPA/DHA** for omega-3 beyond flax. The calcium figures
assume your soy milk and yogurt are **calcium-fortified** — check the carton. Nutrition numbers
are good-faith estimates; verify against your actual product labels, and treat this as a
planning aid, not medical or dietetic advice.

## Licence
MIT — see [LICENSE](LICENSE).
