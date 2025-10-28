# BSE_stock-recommendation

Architecture of the program

                ┌─────────────────────────┐
                │   BSE Announcements     │
                └───────────┬─────────────┘
                            │
                            │ (scrape every run)
                            ▼
                  ┌───────────────────┐
                  │   Scrape Step     │
                  │  `app.py scrape`  │
                  └───────┬───────────┘
                          │
                writes PDFs + metadata
                          │
                          ▼
        ┌─────────────────────────────────────────┐
        │         Local File Storage (data/)       │
        │                                          │
        │  raw/YYYY-MM-DD/*.pdf                    │
        │  meta/YYYY-MM-DD/announcements.jsonl     │
        └─────────────────┬───────────────────────┘
                          │
                          │
                          ▼
                  ┌───────────────────┐
                  │  Score Step       │
                  │ `app.py score`    │
                  │ (LLM API call)    │
                  └───────┬───────────┘
                          │
                  writes per-file JSON judgments:
                  event_type, direction, materiality, confidence, why
                          │
                          ▼
        ┌─────────────────────────────────────────┐
        │    results/YYYY-MM-DD/micro_scores.jsonl│
        └─────────────────┬───────────────────────┘
                          │
                          │
                          ▼
                  ┌───────────────────┐
                  │  Rank Step        │
                  │ `app.py rank`     │
                  │ (simple heuristic)│
                  └───────┬───────────┘
                          │
                outputs Top-K movers lists
                          │
                          ▼
         ┌───────────────────────────────┐
         │   results/YYYY-MM-DD/         │
         │       watchlist_preopen.json  │
         │       watchlist_live.json     │
         └───────────────────────────────┘
