# Reflections — TD1

## 1. Before building anything — personal weighting

For a one-week holiday I would put weather slightly ahead of CO₂, around 60/40: a
low-carbon trip where it rains all week is not really a vacation. But I would not
ignore CO₂ completely — between two destinations with similar weather, I would
clearly pick the closer one.

## 2. Stage 1 — Acquisition reliability

Every connection is a separate chance to fail: 26 TLS handshakes from the same
shared GitHub Actions runner IP means 26 chances for one request to hang, time out
or be throttled by the API, and the run fails as soon as one of them does. The
failures are also not independent — after a few fast requests from the same IP the
API is more likely to start throttling the rest. One batched request means a single
connection to get right, and the retry logic only has to protect one call.

## 3. Stage 1 — Provenance of the CO₂ factor

I would not publish 0.15 as-is. I would take the current official factor from the
source (the latest UK DESNZ/DEFRA conversion factors, or the French ADEME Base
Empreinte), cite the exact table and year next to the number in the code and on the
page, and decide explicitly whether it includes radiative forcing (non-CO₂
effects), since that changes the value a lot. I would also re-check it every year
when the new factors are released.

## 4. Stage 2 — Relative vs. absolute

Adding a very cold and rainy 27th capital would not change a mediocre capital's real
temperature at all, but it would change its score: the minimum of the range moves
down, so the mediocre capital gets pushed closer to 1. The measurement (°C, mm,
kg CO₂) is an absolute fact; the KPI built on it is relative to the set being
compared, so a score only means something inside the list it was computed on.

## 5. Stage 3 — Prediction before running

I predict **Madrid**. With a 50/50 formula the closest capitals (Brussels,
Luxembourg, Amsterdam) win on CO₂ but should have poor late-October weather, while
Valletta, Nicosia and Athens have the best weather but are the farthest; Madrid
(and Rome) sit in the middle, with warm, sunny weather at only about 1,050 km.

## 6. Stage 6 — Was the prediction right?

Yes: the first run from Paris (21 October 2026) ranked **Madrid** first (final score
0.754: weather 0.80, CO₂ 0.71), ahead of Lisbon and Rome. Lisbon actually had better
weather (0.88), but its extra ~400 km cost more on the CO₂ side than it gained. So
the prediction matched because I reasoned with the formula itself: with 50/50
weights, a capital that is "good enough" on both criteria beats one that is
excellent on only one — Brussels had the best CO₂ score (1.0) but only 0.23 for
weather and finished 5th.

## 7. Stage 6 — Monitoring

The only signal on the public page is the "Data last updated …" line: if the date
is more than a week old, the pipeline has stopped or is failing. It is not good
enough on its own, because a visitor has to notice it and know the data is supposed
to be weekly. I would make the page compare `generated_at` to today's date and show
a visible warning when the data is older than 8 days, and enable GitHub's
notifications for failed workflow runs.

## 8. Stage 7 — Changing the origin

I chose **Le Mans** (48.0061, 0.1996) and changed two files:

- `scripts/fetch_and_score.py`: replaced the hardcoded `PARIS_LAT, PARIS_LON` with
  `ORIGIN_NAME`, `ORIGIN_LAT`, `ORIGIN_LON`, used them in the distance calculation
  and in the exclusion filter, and wrote `origin` into `rankings.json`. Le Mans is not
  in `capitals.csv`, so the exclusion removes nothing and Paris becomes a 27th
  destination — the filter is kept so the script still works if the origin is a
  capital.
- `docs/index.html`: the title now reads the origin from `rankings.json` instead of
  saying "Paris", and the chart title uses the real number of destinations instead
  of a hardcoded 26.

These are the right places because the origin is a parameter of the pipeline, not
of the page: the script is the single place it is defined, and the page only
displays what the data says, so changing the origin again means editing three
values and nothing else.

The "best" destination is relative to the starting point. Madrid stayed first from
Le Mans (0.778), but the rest of the ranking moved: Paris, only 185 km away, jumped
to 3rd with the best CO₂ score, and Brussels fell from 5th to 9th because it is now
farther away. Changing the origin also changes the min and max distances used by
`normalize()`, so every CO₂ score shifts even for capitals that are not closer.
