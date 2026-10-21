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
| Visualization | `docs/index.html` — recommendation, ranked bar chart, weather vs CO₂ scatter |
| Automation | `.github/workflows/update-data.yml` — every Monday 06:00 UTC + manual run |

Answers to the TD questions are in [`REFLECTIONS.md`](REFLECTIONS.md).
