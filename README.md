# BSE_stock-recommendation

Architecture of the program

                  ┌──────────────────────────┐
                  │ Scrape Corperate filings │
                  │ from the BSE website     │
                  │  `app.py scrape`         │
                  └───────┬──────────────────┘
                          │
                saves the pdfs in local storage
                          │
                          ▼
        ┌─────────────────────────────────────────────────────────────┐
        │         LLM ranking                                         │
        │                                                             │
        │  takes all this data and sends it to Chatgpt for evaluation │
        │                               GPT.py                        │
        └─────────────────┬───────────────────────────────────────────┘
                          │
                          │
                          ▼
         ┌──────────────────────────────────────────────────────┐
         │   Outputs result and sends it to an email ID         │
         │       EmailUpdater.py                                │
         │                                                      │
         └──────────────────────────────────────────────────────┘
