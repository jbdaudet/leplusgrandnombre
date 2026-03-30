# Geomarketing Application

A geomarketing analysis tool for France that generates catchment-area reports for any address. Built as a Flask web application, it produces KPIs, choropleth maps, and comparison charts across five analysis sections: zone characteristics and pedestrian footfall, population demographics, public transport accessibility, local amenities, and competitive landscape. All metrics are benchmarked against département averages.

## Table of Contents

- [Key Concepts](#key-concepts)
- [Sections](#sections) — Zone, Population, Accessibility, Equipments, Concurrence
- [Services](#services) — Analysis, Catchment, Comparison, Geocoding
- [Data Layer](#data-layer) — Database, OSM Fetcher, SQL Queries
- [Models](#models) — KPI, Section, Analysis
- [Utilities](#utilities) — Custom LLM, Google Places
- [Getting Started](#getting-started) — Prerequisites, Installation, Data Files, Environment Variables, Running, DevContainer
- [API Endpoints](#api-endpoints)
- [Project Structure](#project-structure)
- [Data Sources](#data-sources)
- [References](#references)

## Key Concepts

This application relies on French administrative geography and public datasets. Here are the key terms used throughout the codebase:

- **IRIS** — the finest-grained statistical unit in France (~2,000 inhabitants each). The base unit for population and demographic data. Each IRIS has a type: Habitat (residential), Activité (business), or Divers (parks, forests).
- **Commune** — a municipality. Income data and equipment counts are available at this level.
- **Département** — a regional administrative division (e.g., Paris = 75, Bouches-du-Rhône = 13). Used as the reference area for all benchmarking comparisons.
- **INSEE** — the French national statistics institute, source of population, income, and urban classification data.
- **BPE** (Base Permanente des Équipements) — an INSEE database cataloging all public amenities and services (shops, schools, hospitals, sports facilities) with standardized type codes (e.g., B201 = hypermarket, D101 = general practitioner).
- **Catchment area** (zone de chalandise) — the circular area around a point of interest, sized by urban classification: 750m in city centers, up to 3km in rural areas.

## Sections

### Zone (`app/sections/area.py`)

The Zone section defines the catchment area around a point of interest and computes its key characteristics:
- **Population & Density**: Total population and density metrics for the catchment area
- **Urban Classification**: Automatic classification (Rural, Suburban, Urban) based on INSEE Unités Urbaines
- **IRIS Type Distribution**: Breakdown by zone type (Habitat, Activité, Divers)

#### Footfall Indicator

The section includes a footfall estimation subsection using Origin-Destination weighted betweenness centrality. This approach improves on standard betweenness by modeling actual pedestrian behavior: people walk from home to destinations, not randomly between all points.

- **Methodology**: OD-weighted betweenness centrality — computes pedestrian flows from residential areas (origins) to commercial/transit destinations, rather than generic betweenness between all nodes
- **Data Sources**:
  - Street network from OpenStreetMap via OSMnx
  - IRIS population data (origins weighted by population)
  - Transit stops from OSM (metro, tram, bus, train)
  - Commercial establishments from BPE database
- **Score Composition**:
  - **50% Flow Percentile**: How much pedestrian traffic passes through (percentile rank)
  - **30% Destination Proximity**: Distance to nearby shops and transit (with exponential decay)
  - **20% Connectivity**: Street intersection degree (number of connections)
- **Classification**: Potentiel piéton scale (Très faible → Très élevé)
- **Visualization**: Network map with yellow-to-red gradient showing flow intensity, blue dots for destinations (shops/transit)

### Population (`app/sections/population.py`)

The Population section provides a socio-demographic profile of the catchment area. It computes age distribution (six brackets from 0-14 to 75+), household count and average size, median income with Q1/Q3 quartiles, working-age ratio, and poverty rate. All metrics are compared to département-level reference values and, when available, to historical 2016 data for trend analysis. The section generates five toggleable choropleth maps (population density, median income, household size, youth ratio, senior ratio) at the IRIS level, along with bar charts for age distribution and income distribution.

### Accessibility (`app/sections/accessibility.py`)

The Accessibility section evaluates public transport coverage within the catchment area. It counts metro stations, tram stops, bus stops, and parkings from OpenStreetMap, and train stations (national, regional, local) from the INSEE BPE database. An accessibility score (0-100) is computed using a log-ratio comparison against département density. All transit points are displayed on an interactive map with color-coded markers by transport type, and each count is compared to the expected number at département density for the same area.

### Equipments (`app/sections/equipments.py`)

The Equipments section inventories local amenities from the INSEE BPE database across five categories: commerce, education, healthcare, leisure, and services. Counts are normalized per 10,000 inhabitants for fair comparison with the département reference. A composite amenities score (0-100) is computed using a log-ratio against département equipment density. The section includes a categorized marker map of all facilities, a bar chart comparing per-capita counts by category to the département, and a detailed breakdown table of the top equipment types present.

### Concurrence (`app/sections/concurrence.py`)

The Concurrence section analyzes the competitive landscape around a location for a user-specified brand. It uses an LLM (via the custom LLM wrapper) to identify the top 3 direct competitors and an appropriate search radius based on business type. Competitor stores are then located via the Google Places API, producing KPIs for store count, competitor density per km², average Google rating, and a competition pressure score (0-100). Reviews are summarized by an LLM into strengths and weaknesses per competitor. Results are displayed on a color-coded map and a bar chart showing store distribution by competitor.

## Services

### Analysis Service (`app/services/analysis.py`)

The main orchestrator that ties everything together. Given an address or coordinates, it geocodes the location, builds the catchment area, computes département comparison data and historical trends, then delegates to each section handler. It supports selective section computation and section-specific parameters (e.g., a brand name for the Concurrence section).

### Catchment Service (`app/services/catchment.py`)

Responsible for building the `CatchmentArea` object: it geocodes the input, determines the catchment radius based on urban classification, creates a circular buffer in Lambert-93 projection, and identifies all IRIS zones and communes that intersect the area.

### Comparison Service (`app/services/comparison.py`)

Computes département-level reference data for benchmarking: population density, age ratios, income statistics, equipment counts per capita, transport density, and OSM transit density. It also includes a `TrendService` that computes historical data (2016 vs 2021) for population, income, and poverty rate evolution.

### Geocoding Service (`app/services/geocoding.py`)

Wraps the French government's Base Adresse Nationale API (`api-adresse.data.gouv.fr`) for forward and reverse geocoding. Converts addresses to coordinates (with Shapely Point output) and retrieves INSEE commune codes for any location in France.

## Data Layer

### Database (`app/data/database.py`)

The `DatabaseManager` class provides access to all static datasets via DuckDB. It serves population data, income statistics, equipment inventories, IRIS and commune geometries, urban classification, and département-level aggregates. It also queries a pre-computed OSM transit database for département-level transit density.

### OSM Fetcher (`app/data/osm.py`)

Handles live queries to OpenStreetMap's Overpass API to retrieve metro stations, tram/bus/train stops, and parking facilities within a given radius. Includes retry logic for reliability.

### SQL Queries (`app/data/queries.py`)

Centralizes the SQL query templates used by the DatabaseManager to keep data access logic separate from business logic.

## Models

### KPI Model (`app/models/kpi.py`)

Defines the `KPI` and `KPIValue` data classes that standardize all metrics across sections. Each KPI has a format (number, percentage, currency, ratio, text), comparison indicators (vs département), trend direction (up/down/stable based on historical data), and a granularity level (point, IRIS, or commune).

### Section Model (`app/models/section.py`)

Defines `SectionData` and `Subsection` containers that hold KPIs, maps, charts, tables, evolution items, and metadata for each analysis section. Handles JSON serialization including numpy type conversion.

### Analysis Model (`app/models/analysis.py`)

Defines the `CatchmentArea` (center, radius, IRIS/commune codes, geographic context, urban classification) and `AnalysisResult` (aggregates all section data into a single serializable response).

## Utilities

### Custom LLM (`app/utils/custom_llm.py`)

A LangChain-compatible wrapper that provides a unified interface to LLM providers (currently OpenAI). Used by the Concurrence section to identify competitors and summarize reviews.

### Google Places Client (`app/utils/google_places.py`)

Client for the Google Places API, used to search for competitor stores by name within a radius and retrieve their ratings, review counts, and review texts.

## Getting Started

### Prerequisites

- Python 3.10+
- The data files in `data/` (see [Data Files](#data-files) below)

### Installation

```bash
pip install -r requirements.txt
```

Note: `requirements.txt` covers the main dependencies. You may also need to install `duckdb`, `langchain-core`, and `tenacity` if not already present in your environment.

### Data Files

The `data/` directory is **gitignored** because the files are too large for version control. Before running the application, you need to place the following files in `data/`:

| File | Source | Content |
|---|---|---|
| `iris.gpkg` | IGN IRIS GE | IRIS zone geometries for all of France |
| `communes.gpkg` | IGN Admin Express | Commune boundaries with INSEE codes |
| `population.db` | INSEE RP 2016 & 2021 | Population by age bracket, households, per IRIS |
| `revenus.db` | INSEE Filosofi | Median income, quartiles, poverty rate per commune |
| `equipements.db` | INSEE BPE 2024 | Amenities and services with type codes and coordinates |
| `unites_urbaines.db` | INSEE UU 2020 | Urban classification per commune (drives catchment radius) |
| `osm_transit.db` | OpenStreetMap (pre-computed) | Département-level transit stop counts and densities |

Ask a team member for the current data files, or rebuild them from the raw sources listed above.

### Environment Variables

| Variable | Required | Used by | Description |
|---|---|---|---|
| `FLASK_ENV` | No | `app/config.py` | `production` or `development` (default) |
| `KLP_OPENAI_API_KEY` | For Concurrence section | `app/utils/custom_llm.py` | OpenAI API key for competitor identification and review summarization |
| `GOOGLE_MAPS_API_KEY` | For Concurrence section | `app/utils/google_places.py` | Google Places API key for competitor store lookup |

The core sections (Zone, Population, Accessibility, Equipments) work without any API keys — they rely only on local datasets and the free French government geocoding API. The Concurrence section requires both `KLP_OPENAI_API_KEY` and `GOOGLE_MAPS_API_KEY` to function.

Environment variables can be set in a `local-secrets.env` file at the project root (gitignored). The application loads it automatically on startup via `python-dotenv`.

### Running

```bash
python run.py
```

Open your browser at `http://localhost:5000`, enter a French address or click on the map, and explore the sections.

### DevContainer

The project includes a `.devcontainer/` configuration for VS Code. It uses Docker Compose (`compose.yaml` + `compose.extend.yaml`) to set up the development environment with all dependencies pre-installed. Secrets are loaded via the `devcontainer-init.sh` and `make_env_var_available.sh` scripts.

## API Endpoints

The Flask application exposes the following REST endpoints:

### `POST /api/analyze`

Run a full analysis for all sections (or a subset).

```json
{
  "address": "10 Rue de Rivoli, Paris",
  "sections": ["area", "population"],
  "include_comparison": true,
  "include_trends": true,
  "radius_override": 1000
}
```

You can pass either `address` or `latitude` + `longitude`. All fields except the location are optional.

### `POST /api/section/<section_id>`

Run a single section. Same body as above, plus `"brand": "Carrefour"` for the `concurrence` section.

### `POST /api/geocode`

Geocode a French address. Body: `{"address": "10 Rue de Rivoli, Paris"}`. Returns `latitude`, `longitude`, `formatted_address`.

### `POST /api/catchment-radius`

Get the catchment radius and urban classification for a location. Body: `{"latitude": 48.856, "longitude": 2.352}`.

## Project Structure

```
├── run.py                # Entry point — loads env vars, starts Flask on port 5000
├── requirements.txt      # Python dependencies
├── data/                 # Static data files (gitignored, see Data Files section)
├── app/
│   ├── app.py            # Flask routes and error handlers
│   ├── config.py         # Configuration (paths, catchment radii, section definitions)
│   ├── data/             # Database manager, OSM fetcher, SQL queries
│   ├── models/           # Data classes (KPI, Section, CatchmentArea, AnalysisResult)
│   ├── sections/         # Section handlers (Area, Population, Accessibility, Equipments, Concurrence)
│   ├── services/         # Business logic (Analysis, Catchment, Comparison, Geocoding)
│   ├── utils/            # LLM wrapper, Google Places client
│   └── templates/        # Jinja2 HTML templates
└── .devcontainer/        # VS Code DevContainer configuration
```

## Data Sources

- **INSEE RP** (Recensement de la Population) — Population demographics by IRIS (2016 and 2021 vintages)
- **INSEE Filosofi** — Income distribution and poverty indicators at commune level
- **INSEE BPE** (Base Permanente des Équipements) — Geocoded inventory of all amenities and services in France, with standardized type codes
- **INSEE UU** (Unités Urbaines) — Urban/rural classification per commune, used to determine catchment radius
- **IGN** (Institut Géographique National) — Administrative boundary geometries for IRIS zones and communes
- **OpenStreetMap** — Street network (via OSMnx), live transit stop queries (via Overpass API)
- **Google Places API** — Competitor store locations, ratings, and reviews (Concurrence section only)
- **OpenAI API** — LLM for competitor identification and review summarization (Concurrence section only)

## References

The footfall estimation in the Zone section is a simplified implementation of the methodology described in:

- Sevtsuk, A. (2021). Estimating Pedestrian Flows on Street Networks: Revisiting the Betweenness Index. *Journal of the American Planning Association*, 87(4), 512-526. [DOI: 10.1080/01944363.2020.1864758](https://doi.org/10.1080/01944363.2020.1864758)
