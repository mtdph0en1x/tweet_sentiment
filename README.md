# Trump Tweet Analysis — NLP & Power BI Dashboard

Sentiment and entity analysis of ~82,000 Trump tweets (2009–2026), built as a portfolio project combining a Python NLP pipeline with an interactive Power BI report.

---

## Key Findings

- **Joe Biden** was the most mentioned person (3,823 mentions), followed by Barack Obama (1,786) and Hillary Clinton (1,297)
- Sentiment toward most mentioned individuals was predominantly negative, with **Jack Smith** and **Adam Schiff** showing the highest negative ratios
- Engagement (likes + retweets) was highest for tweets with media
- Overall tweet sentiment declined and became more negative over the years, with a clear trend line

---

## Pipeline Overview

```
Raw tweets → Text cleaning → spaCy NER → Nickname normalization (174 entries)
    → RoBERTa sentiment (per entity) → Star schema export → Power BI
```

---

## Technical Decisions

**RoBERTa over VADER for entity-level sentiment**
VADER operates at tweet level and doesn't distinguish sentiment directed at specific entities. RoBERTa (`cardiffnlp/twitter-roberta-base-sentiment`) was applied per extracted entity mention, giving more granular results. Both models were compared — disagreement was ~8%, concentrated in neutral/negative boundary cases.

**Nickname normalization dictionary**
Trump frequently refers to people by nicknames ("Sleepy Joe", "Crooked Hillary"). A 174-entry normalization dictionary was built from Wikipedia's list of Trump nicknames to map these to canonical names before NER, improving entity coverage significantly.

**Upstream data quality fixes**
NER false positives and duplicate entities were resolved in the Python pipeline rather than in Power BI, keeping the data model clean and transformations reproducible.

---

## Known Limitations

- NER is imperfect — some false positives remain (e.g. "Bill" without surname)
- RoBERTa confidence scores reflect model certainty, not ground-truth sentiment probability
- The ban period (Jan 8 2021 – Nov 19 2022) creates a gap in the timeline by design

---

## Stack

| Layer | Tools |
|---|---|
| NLP | spaCy, HuggingFace Transformers (RoBERTa), VADER |
| Data | pandas |
| Visualization | Power BI, DAX |
| Environment | CUDA (RTX 4070), Jupyter |

---

## Dashboard Pages

1. **Sentiment Overview** — tweet-level sentiment trends over time
2. **Entity Analysis** — top mentioned people, orgs, locations with sentiment breakdown
3. **Engagement Patterns** — likes/retweets by sentiment, time, entity
4. **Posting Behavior** — activity by hour, day, period

---

*Data source: [Trump Tweets Dataset](https://datadrivendecisionlab.com/resources?resource=trump-tweets-dataset). Project built for portfolio purposes.*
