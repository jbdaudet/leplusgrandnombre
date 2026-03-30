# The Shift Project — Energy Analytics Dashboard

## Overview

An AI-powered conversational analytics platform for energy and climate data. Users ask questions in natural language; the system generates SQL, fetches data, renders visualizations, and provides expert analysis in the voice of a Jancovici-inspired energy analyst.

**Live**: https://theshiftproject.leplusgrandnombre.fr

### Key Features

- **Semantic Layer**: Metrics catalog (YAML) with deterministic SQL assembly — LLM picks metric + dimensions + filters, no SQL generation needed for standard queries
- **Text-to-SQL Fallback**: For novel queries not covered by the catalog, single LLM call generates PostgreSQL, validated with EXPLAIN before execution
- **Multi-Agent Pipeline**: Classification, data extraction, strategic planning, corpus RAG, and analysis
- **Jancovici Persona**: Style, vocabulary, convictions, and anti-lexicon derived from 10 transcribed interviews via BERTopic + AI
- **Streaming Responses**: SSE streaming for all user-facing agents
- **Vega-Lite Visualizations**: Auto chart selection with full Vega-Lite specs (bar, line, LLM-generated); KPI cards and tables via custom HTML
- **Sectoral Data**: Energy consumption by sector (Industry, Transport, Residential) from Eurostat + UNSD energy balances
- **pgvector RAG**: 4-stage cascading retrieval with conviction/topic pre-filtering
- **Virtual KPIs**: Computed metrics (energy intensity, emissions per capita, etc.) defined declaratively in the semantic layer
- **Auth**: Login, admin panel with invite links, user profiles

---

## Agent Flow

```
User Question
     |
     v
Classification Agent (Haiku)
     |
     |--- TACTICAL ---> Semantic Layer (Haiku picks metric) ---> Deterministic SQL ---> Viz ---> Strategist
     |                  |                                                                        (streaming)
     |                  +--- fallback ---> Text-to-SQL (Sonnet) ---> EXPLAIN ---> Viz ---> Strategist
     |                  (Sonnet)          |        (Sonnet + corpus)
     |                                   +--- Comparison (Haiku, parallel)
     |
     |--- STRATEGIC --> ClarificationAgent (streaming) ---> Sub-questions ---> Data + Strategist
     |                  (multi-step: diagnosis -> vision -> trade-off -> roadmap)
     |
     |--- CORPUS -----> Persona + Corpus RAG (streaming) ---> Response (no data extraction)
     |                  (e.g. "What is Mtoe?", "What does Jancovici think about nuclear?")
     |
     |--- METADATA ---> Data dictionary lookup ---> Response
     |
     +--- REJECTED ---> Polite redirect (Haiku, streaming) ---> Response
```

---

## Data Sources

| Notebook | Source | API | Target Table | Coverage |
|----------|--------|-----|-------------|----------|
| `01_eia.ipynb` | U.S. EIA | `api.eia.gov/v2` (free key) | `energy_production`, `energy_consumption` (Total) | 220 countries, 1980-2024 |
| `02_unsd.ipynb` | UN Statistics | SDMX API (free) | `energy_flow` (supply, transformation, consumption by sector) | 232 countries, 1990-2024 |
| `03_pik_primap.ipynb` | PIK/PRIMAP-hist | Zenodo (free) | `ghg_emissions` (4 gases: KYOTOGHG, CO2, CH4, N2O) | 207 countries, 1990-2024 |
| `04_population_gdp.ipynb` | World Bank | API (free) | `population`, `gdp` | 220 countries, 1960-2024 |
| `05_eurostat_sectors.ipynb` | Eurostat | JSON API (free) | `energy_consumption` (by sector) | 38 EU/EEA countries, 1990-2024 |

### Data Dictionary

Managed via Excel: edit `setup/dictionary/data_dictionary.xlsx`, then run `python setup/03_export_dictionary.py to-json` to rebuild JSON files. Dimension variables support both flat lists and annotated dicts (value -> explanation) in the `values` column.

**Virtual KPIs**: Computed metrics (energy intensity, emissions per capita, etc.) are defined in `setup/metrics.yaml` as part of the semantic layer. The LLM picks the metric, and SQL is assembled deterministically — no need to store formulas in the data dictionary.

---

## Corpus & Persona

### Jancovici Persona Pipeline

1. **Transcription**: 10 YouTube interviews transcribed via faster-whisper (medium, int8, ~1.6x realtime)
2. **Segmentation**: Transcripts split into ~700-word sub-documents (~270 segments)
3. **Topic Discovery**: AI-generated topic taxonomy (15 topics) from segment analysis
4. **Conviction Discovery**: BERTopic clustering -> 34 convictions labeled by AI (generate + judge pattern)
5. **Style Analysis**: Corpus-derived lexicon, connectors, verbal tics, anti-lexicon
6. **Embedding + Seeding**: 789 chunks embedded via HuggingFace Inference API, stored in pgvector

### RAG Architecture

- **Classifier**: Query -> HyDE (hypothetical Jancovici response) -> cosine similarity against pre-embedded topics/convictions
- **Retriever**: 4-stage cascade (conviction+topic -> conviction -> topic -> all), pgvector cosine search
- **Context**: Static persona (bio, style, vocab) + dynamic per-query (relevant convictions + corpus chunks)
- **Single source**: `CorpusRetriever.get_full_context()` serves all agents identically

---

## Data Visualization

### Architecture

Visualizations are rendered client-side using **Vega-Lite** (bar, line, and LLM-generated charts) or **custom HTML** (KPI cards, tables, lists). The backend builds a complete Vega-Lite JSON spec and sends it to the frontend, which renders it in one call via `vegaEmbed()`.

### Visualization Pipeline

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                        choose_visualization()                               │
│                  data_extraction_tool.py                                     │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│   DataFrames + MetricSpec                                                   │
│        │                                                                    │
│        ▼                                                                    │
│   ┌──────────────────────┐                                                  │
│   │  Detect data shape   │                                                  │
│   └──────────┬───────────┘                                                  │
│              │                                                              │
│    ┌─────────┼─────────┬───────────┬───────────┐                            │
│    ▼         ▼         ▼           ▼           ▼                            │
│  1 row    time dim   categ.    2+ dims     other                            │
│  ≤2 col  + numeric   + num.   (complex)   (fallback)                       │
│    │         │         │           │           │                            │
│    ▼         ▼         ▼           ▼           ▼                            │
│ KPI card   Line      Bar        Table    LLM Vega-Lite                     │
│ (HTML)   (Vega-Lite) (Vega-Lite) (HTML)  (Haiku gen)                       │
│    │         │         │           │        │                               │
│    │         │         │           │        ├── success ──▶ Vega-Lite spec  │
│    │         │         │           │        └── fail ─────▶ Table (HTML)    │
│    │         │         │           │                                        │
│    ▼         ▼         ▼           ▼                                        │
│  DataSpec  DataSpec  DataSpec   DataSpec                                    │
│  (no VL)  (+VL spec)(+VL spec) (no VL)                                     │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

### Spec Generation (backend)

```
┌───────────────────────────────────────────────────────────────────────┐
│                     vega_lite_builder.py                               │
├───────────────────────────────────────────────────────────────────────┤
│                                                                       │
│  build_bar_spec()                   build_line_spec()                 │
│  ┌─────────────────────┐            ┌─────────────────────┐          │
│  │ • Auto horizontal   │            │ • Multi-series via  │          │
│  │   if 7+ categories  │            │   color encoding    │          │
│  │ • Grouped bars for  │            │ • Monotone interp.  │          │
│  │   comparison series │            │ • Point marks       │          │
│  │ • Data labels layer │            │ • Zero-free Y axis  │          │
│  │ • Sorted by value   │            │                     │          │
│  └─────────────────────┘            └─────────────────────┘          │
│                                                                       │
│  generate_vega_lite_spec_llm()     Brand constants                    │
│  ┌─────────────────────┐           ┌─────────────────────┐           │
│  │ • Haiku generates   │           │ COLOR_PRIMARY #1976d2│           │
│  │   full VL spec      │           │ 10-color palette    │           │
│  │ • Data injected     │           │ Segoe UI font       │           │
│  │   post-generation   │           │ Shared axis/legend  │           │
│  │ • Schema validated  │           │ config fragments    │           │
│  └─────────────────────┘           └─────────────────────┘           │
│                                                                       │
└───────────────────────────────────────────────────────────────────────┘
```

### Rendering (frontend)

```
┌───────────────────────────────────────────────────────────────────────┐
│                    renderVisualization.js                              │
├───────────────────────────────────────────────────────────────────────┤
│                                                                       │
│  payload from backend                                                 │
│       │                                                               │
│       ▼                                                               │
│  vega_lite_spec present?                                              │
│       │                                                               │
│    ┌──┴──┐                                                            │
│   yes    no                                                           │
│    │      │                                                           │
│    ▼      ▼                                                           │
│  vegaEmbed()        chart_type?                                       │
│  ┌────────────┐     ┌──────┬──────┬───────┬──────┐                   │
│  │ SVG render │     │kpi   │table │list   │other │                   │
│  │ Tooltips   │     │card  │      │       │      │                   │
│  │ Export menu│     ▼      ▼      ▼       ▼      │                   │
│  │ Responsive │   Custom HTML templates  Table   │                   │
│  └────────────┘   (formatKPI, humanize)  fallback│                   │
│                                                                       │
│  Libraries: Vega 5 + Vega-Lite 5 + Vega-Embed 6                     │
│                                                                       │
└───────────────────────────────────────────────────────────────────────┘
```

### End-to-End Data Flow

```
  SQL results (DataFrames)
        │
        ▼
  choose_visualization()  ─────▶  DataSpec { data, chart_type, vega_lite_spec? }
        │
        ▼
  DataSpec.to_dict()  ─────────▶  { sql: {...}, data: [...], viz: {...}, vega_lite_spec: {...} }
        │
        ▼
  chat_service.py  ────────────▶  JSON payload via SSE stream
        │
        ▼
  index.html JS  ──────────────▶  renderVisualization(payloadItem)
        │
        ├── vega_lite_spec?  ──▶  vegaEmbed('#el', spec)  →  SVG chart
        └── else  ─────────────▶  Custom HTML  →  KPI card / table / list
```

### Chart Type Decision Matrix

| Data Shape | Chart Type | Renderer | Builder |
|---|---|---|---|
| 1 row, ≤2 cols, numeric | KPI card | Custom HTML | — |
| 1 row, text | List | Custom HTML | — |
| Time dim + numeric + comparison | Line | Vega-Lite | `build_line_spec()` |
| Time dim + categorical (multi-series) | Line | Vega-Lite | `build_line_spec()` |
| Categorical + numeric + comparison | Bar | Vega-Lite | `build_bar_spec()` |
| Categorical + numeric (no comparison) | Bar | Vega-Lite | `build_bar_spec()` |
| 2+ dimensions | Table | Custom HTML | — |
| Anything else | LLM-generated | Vega-Lite | `generate_vega_lite_spec_llm()` |
| LLM generation fails | Table fallback | Custom HTML | — |

### Key Files

| File | Purpose |
|---|---|
| `backend/core/agent_tools/vega_lite_builder.py` | Deterministic spec builders + LLM fallback |
| `backend/core/agent_tools/data_extraction_tool.py` | `choose_visualization()`, `DataSpec` |
| `frontend/static/renderVisualization.js` | Vega-Lite + HTML rendering |
| `frontend/index.html` | Vega/Vega-Lite/Vega-Embed CDN |

---

## User Feedback & Pipeline Telemetry

Users can thumbs-up / thumbs-down any answer. Votes are stored in the `pulse_ai.sessions` table (`vote` column: 1 = good, -1 = bad, 0 = no vote).

Every answer payload includes a `_pipeline` metadata block:

```json
{
  "_pipeline": {
    "path": "semantic",
    "metric": "energy_per_capita",
    "filters": {"country_code": "FRA", "year": 2023}
  }
}
```

| Field | Values | Description |
|-------|--------|-------------|
| `path` | `"semantic"` or `"text-to-sql"` | Which pipeline produced the answer |
| `metric` | metric name or `null` | Semantic layer metric selected (null for text-to-SQL) |
| `filters` | object or `null` | Filters applied by the semantic layer |

### Querying feedback

```sql
-- Downvotes by pipeline path
SELECT payload::json->'_pipeline'->>'path' AS pipeline, COUNT(*)
FROM pulse_ai.sessions
WHERE vote = -1
GROUP BY 1;

-- Most downvoted metrics
SELECT payload::json->'_pipeline'->>'metric' AS metric, COUNT(*)
FROM pulse_ai.sessions
WHERE vote = -1 AND payload::json->'_pipeline'->>'path' = 'semantic'
GROUP BY 1 ORDER BY 2 DESC;
```

---

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Frontend | Static HTML/CSS/JS, Vega-Lite 5, marked.js |
| Backend | Flask, Gunicorn |
| AI | Anthropic Claude (Sonnet for generation, Haiku for classification) |
| Database | PostgreSQL 15 + pgvector |
| Embeddings | HuggingFace Inference API (384-dim, multilingual) |
| Deployment | Docker (ARM64), AWS ECR + EC2 + Caddy, GitHub Actions CI/CD |

---

## Local Development

```bash
# Start PostgreSQL (devcontainer does this automatically)
pg_ctlcluster 15 main start

# Run setup (first time)
python setup/01_explore_schema.py
python setup/02_build_dictionary.py

# Start the app
bash start-app.sh   # or: python -m backend.app
```

---

## Deployment

See `INFRA_README.md` for full infrastructure documentation. Quick summary:

- **URL**: https://theshiftproject.leplusgrandnombre.fr
- **Infra**: AWS EC2 t4g.small (ARM64) + RDS db.t4g.micro + Caddy reverse proxy
- **CI/CD**: Push to `main` -> GitHub Actions builds ARM64 image -> ECR -> SSH deploy
- **Secrets**: AWS SSM Parameter Store (`/theshiftproject/*`)
- **Cost**: ~1 EUR/month marginal (ECR only, DB and EC2 shared)

---

## Configuration

### `setup/client.yaml`

Brand, profiles, platform context, and question examples for the classifier.

### `setup/db_connection.yaml`

Database type and env var names for credentials.

### `setup/dictionary/data_dictionary.xlsx`

Single source of truth for table descriptions, column metadata, dimension values. Edit here, then `python setup/03_export_dictionary.py to-json`.

### Environment Variables

| Variable | Purpose |
|----------|---------|
| `PG_SERVER` | PostgreSQL host |
| `PG_DATABASE` | Database name (`ww_energy_db`) |
| `PG_USERNAME` | DB user |
| `PG_PASSWORD` | DB password |
| `AI_DASHBOARD_ANTHROPIC_API_KEY` | Claude API key |
| `HF_API_TOKEN` | HuggingFace Inference API token |
| `SECRET_KEY` | Flask session signing |
