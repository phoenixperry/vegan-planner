# THRUM · Vegan Plate

A mobile-first weekly meal planner for a **high-protein, high-calcium vegan cut**.
Every generated day is constrained to **1200–1400 kcal**, **≥107 g protein** (1.6 g/kg
at 67 kg), and pushed toward the **1000 mg calcium** RDA. Two daily flat whites
(480 ml fortified soy milk) are pinned into every day automatically.

No build step, no dependencies, no backend. One `index.html`.

## Features
- **Today** — the day's plate (coffee + breakfast + lunch + dinner ± one snack), live macro
  bars vs target, swipe or tap arrows to move between days, per-meal `swap`.
- **Shop** — the whole week's ingredients aggregated by aisle, tickable, copy-to-clipboard.
- **Plan** — generate a fresh week, or reroll a single day, from the built-in meal database.

## How the generator works
Each day is solved by **exhaustive search**, not random sampling. It enumerates every
`breakfast × lunch × dinner × (no snack | one of 7 snacks)` combination
(5 × 6 × 6 × 8 = 1440), keeps only those that land inside the kcal band **and** clear the
protein floor, then prefers combinations whose mains weren't used on the previous day for
variety. If no in-band combination meets the variety constraint it relaxes variety before it
relaxes the nutrition targets — the macros are never sacrificed. Validated over 300 simulated
weeks: 100% of days in-band, 100% ≥107 g protein, ~13 distinct mains/week.

Edit the `DB` object near the top of the `<script>` block to add or retune meals
(`kcal` / `p` grams protein / `ca` mg calcium / `ing` shopping ingredients).

## Run it
Open `index.html` in any browser, or serve the folder:
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
