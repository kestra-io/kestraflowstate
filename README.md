# kestraflowstate

Kestra hackathon starter flows built on the Jaffle Shop dataset. Flows live in [`flows/`](flows/).

| File | Flow ID | What it does |
|---|---|---|
| `flows/jaffle_shop_insights_kestra_hackathon.yaml` | `jaffle_shop_insights_kestra_hackathon` | Downloads the dataset, analyzes it with DuckDB, logs a business overview and store leaderboard, outputs a product profitability CSV. No setup needed. |
| `flows/jaffle_shop_insights_kestra_hackathon_ai.yaml` | `jaffle_shop_insights_kestra_hackathon_ai` | Compares two sales periods in DuckDB, asks Gemini for a briefing and a structured alert level, then branches on it. Needs `GEMINI_API_KEY` in the namespace KV Store. |

To sync them into Kestra, point `io.kestra.plugin.git.SyncFlows` at this repo with `gitDirectory: flows`.
