# EU capitals — sustainable vacation dashboard

TD1 — Business Intelligence.

**Decision supported:** which EU capital offers the best vacation conditions next
week while minimizing the CO₂ emissions of getting there?

## How it works

| Stage | Where |
|---|---|
| Acquisition | `data/capitals.csv` (static coordinates) + [Open-Meteo](https://open-meteo.com) 7-day forecast (live, no API key) |
| Transformation | `scripts/fetch_and_score.py` — haversine distance, min-max normalization |
| KPI | `final_score` = 50 % weather score + 50 % CO₂ score |
| Storage | `docs/data/rankings.json`, committed on every run (history kept in Git) |
| Visualization | `docs/index.html` — recommendation card, map, weight slider, stacked ranking bars, weather vs CO₂ trade-off, sortable table |
| Automation | `.github/workflows/update-data.yml` — every Monday 06:00 UTC + manual run |

Answers to the TD questions are in [`REFLECTIONS.md`](REFLECTIONS.md).

## Run it locally

```bash
python3 serve.py            # opens the dashboard on http://localhost:8000
python3 serve.py --refresh  # recompute the ranking first (pip3 install requests)
```
