# Rent-A-Wreck — Operations Dashboard

A single-file HTML dashboard (`index.html`) presenting fleet & rental-agreement
data for a car rental business. No build system, no dependencies beyond Google
Fonts. Just open `index.html` in a browser, or serve the folder over HTTP.

## Hosting

- **Repo:** https://github.com/jcitizen-dev/rent-a-wreck (public)
- **Live site (GitHub Pages):** https://jcitizen-dev.github.io/rent-a-wreck/
- Pages deploys from the `main` branch, `/` (root). If the site 404s after the
  repo visibility was toggled, Pages was auto-disabled — re-enable it under
  Settings → Pages → Source: "Deploy from a branch", `main` / root.

## Architecture

Everything is in `index.html`:
- **Theme:** CSS custom properties (`--bg`, `--gold`, `--brand-red`, etc.).
  Currently light mode with a red company brand color (`--brand-red: #c41212`).
- **Data:** two JS arrays near the bottom of the file —
  - `fleet` — every vehicle (`unit`, `cls`, `year`, `model`, `body`, `plate`,
    `color`, `mileage`, `status` ('rent' | 'avail'), `tb`, `inSvc`).
  - `agreements` — open rental contracts (renter name, RA#, unit, agent, rate,
    contract type).
- **Counts are dynamic** — KPIs, the status board, and the overview subtitle all
  derive from `fleet.length` and per-status filtering. Do NOT hardcode totals.
- **Fleet tab** uses a fixed-height inner-scroll container so `thead th` can be
  `position:sticky; top:0`. `#tab-fleet` only gets `display:flex` when it also
  has `.active` (avoids an ID-vs-class specificity bug that hid other tabs).
- **Charts** are hand-built SVG (a half-donut gauge and a donut). The donut uses
  `stroke-dasharray` + `stroke-dashoffset` with a single `rotate(-90)` — don't
  double-apply the rotation in the offset math.

## Data reconciliation status

Fleet data was reconciled against printed source documents (physical inventory
reports). Current state:

- **Total in `fleet` array: 105.** Printed inventory shows **107 total** — there
  are **2 units still missing** (1 FCAR, 1 ICAR) that weren't legible in the
  source photos. Add them if a cleaner export turns up.
- Per-class counts (match the docs): CCAR 67, ECAR 1, FCAR 9, HBRD 1, ICAR 17,
  LCAR 1, MVAN 1, XFAR 8.
- **25 open agreements** — matches the rental-agreements report.

Recent corrections made from the source docs (already committed/pushed):
- Unit 615 (A6, ICAR): status `rent` → `avail`.
- Unit 19 (1995 Camry): class `ECAR` → `FCAR`.
- Unit 87 (2002 Altima): class `XFAR` → `FCAR`.

## Conventions

- Edit `index.html` directly; verify by opening it in a browser.
- This started as a "Marathon / RAW" dashboard and was rebranded to
  "Rent-A-Wreck" — there should be **no remaining references to "Marathon" or
  "RAW"** in user-facing text. Watch for these when adding content.
