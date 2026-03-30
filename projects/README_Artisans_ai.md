# Artisan.ai

A Flask-based web application for French artisans to manage their projects, clients, stock, quotes, and planning -- with AI-powered assistance.

Built for 17 supported trades (Menuisier/Ebeniste, Charpentier, Plombier, Electricien, Macon, Peintre, Carreleur, Platrier, Couvreur, Serrurier, Metallier, Soudeur, Mecanicien, Carrossier, Vitrier, Tapissier, Marbrier).

---

## Architecture Overview

The app runs in two environments — a local development setup and a production deployment on AWS. Code flows from local to production via GitHub Actions.

```
┌─────────────────────────────────────────────────────────────────────────┐
│  LOCAL (Development)                                                    │
│                                                                         │
│  docker-compose up                                                      │
│  ┌──────────────────┐    ┌────────────────┐                             │
│  │  Flask dev server │    │  SQLite         │                            │
│  │  port 5000        │───▶│  artisans_dev.db│                            │
│  │  hot reload       │    └────────────────┘                             │
│  └──────────────────┘                                                   │
│  local-secrets.env (API keys)                                           │
└─────────┬───────────────────────────────────────────────────────────────┘
          │
          │  git push origin main
          ▼
┌─────────────────────────────────────────────────────────────────────────┐
│  GITHUB ACTIONS (CI/CD)                                                 │
│                                                                         │
│  1. Build Docker image (Dockerfile.prod)                                │
│  2. Push to ECR                                                         │
│  3. Deploy new ECS task definition                                      │
│  4. Wait for service stability                                          │
│  5. Run flask db upgrade (migrations)                                   │
│  6. Run flask promote-admin (from ADMIN_EMAIL secret)                   │
└─────────┬───────────────────────────────────────────────────────────────┘
          │
          ▼
┌─────────────────────────────────────────────────────────────────────────┐
│  AWS (Production)                               Region: eu-west-3       │
│                                                                         │
│  ┌─────────┐    ┌──────────────────┐    ┌──────────────────┐            │
│  │  Route53 │───▶│  ALB (HTTPS)     │───▶│  ECS Fargate     │            │
│  │  DNS     │    │  ACM certificate │    │  Gunicorn × 4    │            │
│  └─────────┘    └──────────────────┘    │  Dockerfile.prod │            │
│                                          └────────┬─────────┘            │
│                                                   │                      │
│                              ┌─────────────────┐  │  ┌────────────────┐  │
│                              │  EFS             │◀─┼─▶│  RDS PostgreSQL│  │
│                              │  uploads/        │  │  │  db.t4g.micro  │  │
│                              │  gammes_usinage/ │  │  └────────────────┘  │
│                              └─────────────────┘  │                      │
│                                                   │                      │
│              ┌────────────────────┐    ┌───────────┴──────┐              │
│              │  SSM Parameters     │    │  ECR              │             │
│              │  SECRET_KEY         │    │  artisans-app     │             │
│              │  DATABASE_URL       │    │  Docker images    │             │
│              │  ANTHROPIC_API_KEY  │    └──────────────────┘              │
│              └────────────────────┘                                      │
└─────────────────────────────────────────────────────────────────────────┘
```

### Local Development

| Component | Details |
|-----------|---------|
| **Server** | Flask dev server with hot reload (port 5000) |
| **Database** | SQLite (`artisans_dev.db`) — file-based, zero setup |
| **Secrets** | `local-secrets.env` file (API keys, not committed) |
| **Container** | `docker-compose up` using `Dockerfile` |
| **Registration** | Open by default (set `REGISTRATION_OPEN=true` in env) |

### Production (AWS)

| Component | Details |
|-----------|---------|
| **Server** | Gunicorn with 4 workers (port 8000) behind ALB |
| **Database** | PostgreSQL 15 on RDS (`db.t4g.micro`) |
| **Secrets** | AWS SSM Parameter Store (SecureString) |
| **Container** | ECS Fargate using `Dockerfile.prod` |
| **Storage** | EFS for persistent uploads and generated files |
| **HTTPS** | ALB + ACM certificate on `artisans.leplusgrandnombre.fr` |
| **CI/CD** | GitHub Actions — push to `main` triggers build + deploy |
| **Registration** | Invitation-only (admin creates codes from `/admin/invitations`) |

### How They Communicate

The two environments share the same codebase but have **separate databases and no data sync**. The connection between them is the deployment pipeline:

1. You develop and test locally against SQLite
2. You `git push origin main`
3. GitHub Actions builds the Docker image, pushes it to ECR, and deploys a new ECS task
4. After deploy, migrations run automatically (`flask db upgrade`) and the admin user is ensured (`flask promote-admin`)
5. The production app starts serving traffic with the updated code

Database migrations are the bridge: Alembic migration files (in `migrations/`) are generated locally and executed on production during deploy.

### Invitation System

Registration is gated by a `REGISTRATION_OPEN` config flag (default: `false`). When closed:
- New users must enter a valid invitation code to register
- Admin creates codes from `/admin/invitations` (shield icon in nav)
- Each code is single-use, 8-char alphanumeric, and can be revoked
- The login page hides the "Créer un compte" link

Set `REGISTRATION_OPEN=true` as an environment variable to re-enable open registration.

---

## Features

- **Client Management** -- Create and manage your client directory with contact info, notes, and linked projects/quotes. Inline client selector with search-as-you-type, full list on focus, and quick inline creation
- **Project Management** -- Create projects manually or via an AI-driven conversation that clarifies scope, materials, specs, and installation needs. Guided metro-line stepper (Description > Conversation IA > Finalisation)
- **Machining Sequences (Gammes)** -- AI-generated step-by-step work instructions with tools, consumables, durations, and costs. Each step is categorized (conception/fabrication/installation) for automatic quote grouping
- **Stock Management** -- Track tools, consumables, and chutes (wood scraps) organized by category and subcategory. Chutes support dimension specs (longueur, largeur, epaisseur). Debounced quantity updates. Automatic stock adjustment when completing project steps. AI-powered stock template generation via 6-step pipeline with real-time SSE progress streaming
- **Feuilles de Débit (Cut Lists)** -- Upload multiple cut lists per project (PDF, images, Excel). AI-powered parsing extracts structured piece data (name, essence, dimensions, quantity). Automatic stock matching via first-fit-decreasing bin-packing shows which pieces are available and which need purchasing. Per-feuille results with an overall stock usage summary
- **Quote Generation (Devis)** -- Generate professional quotes from completed project steps. Steps are grouped into 3 categories (Analyse et conception, Fabrication en atelier, Installation sur site) plus optional lines for consumables and travel. Supports deposit (acompte) with configurable percentage. Clean PDF export with light gray/black palette, signature space, and deposit mention
- **Planning & Calendar** -- Visual calendar with auto-scheduled project steps (skips weekends)
- **Shop Integration** -- Configure preferred suppliers with smart search patterns; includes 21 pre-seeded French shops with autocomplete
- **AI Pattern Discovery** -- AI can detect shop search URL patterns automatically
- **Multi-Provider AI** -- Switch between OpenAI, Anthropic, Mistral, Ollama, or HuggingFace without code changes
- **User Authentication** -- Registration, login, profile management with logo upload, billing info, danger zone
- **Invitation System** -- Admin-only invitation codes for gated registration, toggleable via `REGISTRATION_OPEN` config flag

---

## Database Schema

```mermaid
erDiagram
    users {
        int id PK
        varchar email UK
        varchar password_hash
        varchar name
        varchar profession
        boolean optin_email
        boolean is_admin
        text pro_address
        varchar siren_number
        decimal hourly_rate
        varchar logo_filename
        varchar shop1_name
        varchar shop1_url
        varchar shop1_search_pattern
        varchar shop2_name
        varchar shop2_url
        varchar shop2_search_pattern
        varchar shop3_name
        varchar shop3_url
        varchar shop3_search_pattern
        datetime created_at
        datetime updated_at
    }

    invitations {
        int id PK
        varchar code UK
        int created_by FK
        int used_by FK
        boolean is_active
        datetime created_at
        datetime used_at
    }

    clients {
        int id PK
        int user_id FK
        varchar name
        text address
        varchar email
        varchar phone
        varchar contact_person
        text notes
        datetime created_at
        datetime updated_at
    }

    projects {
        int id PK
        int user_id FK
        int client_id FK
        varchar label
        text description
        date start_date
        varchar status
        varchar client_name
        text client_address
        json conversation_data
        datetime created_at
        datetime updated_at
    }

    feuilles_debit {
        int id PK
        int project_id FK
        varchar label
        varchar filename
        varchar original_filename
        json parsed_data
        datetime created_at
    }

    conversations {
        int id PK
        varchar conversation_id UK
        varchar user_id FK
        json state
        datetime created_at
        datetime updated_at
    }

    machining_steps {
        int id PK
        int project_id FK
        float step_number
        varchar name
        text description
        varchar duration
        varchar status
        varchar quote_category
        date planned_start
        date planned_end
        decimal unit_cost
        decimal material_cost
        decimal total_cost
        json consumables
        json tools
        datetime created_at
        datetime updated_at
    }

    stock {
        int id PK
        int user_id FK
        varchar item_name
        varchar category
        varchar subcategory
        varchar type
        json specs
        int quantity
        decimal unit_price
        datetime last_updated
        datetime created_at
    }

    quotes {
        int id PK
        varchar quote_number UK
        int user_id FK
        int project_id FK
        int client_id FK
        date quote_date
        date valid_until
        date sent_date
        date accepted_date
        varchar status
        varchar client_name
        text client_address
        decimal subtotal
        decimal tax_rate
        decimal tax_amount
        decimal total
        decimal discount_percent
        decimal discount_amount
        boolean deposit_enabled
        decimal deposit_percent
        text notes
        datetime created_at
        datetime updated_at
    }

    quote_lines {
        int id PK
        int quote_id FK
        int step_id FK
        int line_number
        text description
        decimal quantity
        varchar unit
        decimal unit_price
        decimal total
        datetime created_at
    }

    default_stock {
        int id PK
        varchar profession
        varchar item_name
        varchar category
        varchar subcategory
        varchar type
        json specs
        int quantity
        datetime created_at
        datetime updated_at
    }

    shops {
        int id PK
        varchar name
        varchar url
        varchar search_pattern
        varchar category
        varchar logo
        boolean is_active
        datetime created_at
        datetime updated_at
    }

    users ||--o{ invitations : "creates"
    users ||--o{ clients : "owns"
    users ||--o{ projects : "owns"
    users ||--o{ stock : "owns"
    users ||--o{ quotes : "creates"
    clients ||--o{ projects : "has"
    clients ||--o{ quotes : "receives"
    projects ||--o{ feuilles_debit : "has"
    projects ||--o{ machining_steps : "contains"
    projects ||--o{ quotes : "generates"
    users ||--o{ conversations : "owns"
    quotes ||--o{ quote_lines : "contains"
    machining_steps ||--o{ quote_lines : "maps to"
```

---

## Quick Start

### Prerequisites

- Docker and Docker Compose
- An API key for your chosen LLM provider (or use local Ollama for free)

### Configuration

Create a `local-secrets.env` file in the project root:

```bash
# LLM Provider Configuration
LLM_PROVIDER=anthropic       # Options: openai, anthropic, mistral, ollama, huggingface
LLM_MODEL=claude-sonnet-4-5
LLM_TEMPERATURE=0.7
LLM_MAX_TOKENS=4000

# API Keys (only the one matching your provider is required)
ANTHROPIC_API_KEY=sk-ant-your-key-here
# ARTISAN_OPENAI_API_KEY=sk-your-key-here
# MISTRAL_API_KEY=your-key-here
# HUGGINGFACE_API_KEY=hf-your-key-here
```

### Launch

```bash
docker-compose up
```

The app is available at **http://localhost:5000**.

### Verify LLM setup

```bash
python tools/test_llm.py
```

---

## Production Deployment

The project includes a production-ready Dockerfile (`Dockerfile.prod`) optimized for secure, performant deployment.

### What `Dockerfile.prod` does

1. **Base image** -- `python:3.12-slim-bookworm` (minimal Debian)
2. **Installs dependencies** -- pip packages from `requirements.txt` + gunicorn
3. **Copies application code** into the container
4. **Creates directories** -- `uploads/` and `gammes_usinage/` for runtime data
5. **Creates a non-root user** (`appuser`, UID 1000) -- the application never runs as root
6. **Exposes port 8000**
7. **Runs gunicorn** with 4 workers and 120s timeout via `wsgi.py`

### Production entrypoint (`wsgi.py`)

```python
app = create_app(os.getenv('FLASK_ENV', 'production'))
```

When `FLASK_ENV` is not set (or set to `production`), the app uses `ProductionConfig`:
- `DEBUG = False`
- Database via `DATABASE_URL` env var (PostgreSQL expected)
- Secure session cookies (`Secure`, `HTTPOnly`, `SameSite=Lax`)
- Security headers on all responses (HSTS, CSP, X-Frame-Options DENY, etc.)

### Build and run for production

```bash
# Build the production image
docker build -f Dockerfile.prod -t artisans-manager:prod .

# Run with production environment variables
docker run -d \
  -p 8000:8000 \
  -e SECRET_KEY=$(python -c "import secrets; print(secrets.token_hex(32))") \
  -e DATABASE_URL=postgresql://user:pass@db-host:5432/artisans \
  -e LLM_PROVIDER=openai \
  -e ARTISAN_OPENAI_API_KEY=sk-your-key \
  artisans-manager:prod
```

### Production checklist

| Item | Details |
|------|---------|
| **Database** | Use PostgreSQL (`psycopg2-binary` is included). Set `DATABASE_URL` env var |
| **Secret key** | Generate a strong random key: `python -c "import secrets; print(secrets.token_hex(32))"` |
| **HTTPS** | Terminate TLS at your reverse proxy (nginx, ALB, etc.). The app sets HSTS headers |
| **Migrations** | Run `flask db upgrade` before starting the app to apply pending migrations |
| **PDF generation** | WeasyPrint requires `libpango-1.0-0` and `libpangoft2-1.0-0` (included in the Debian base) |
| **Workers** | Gunicorn runs 4 workers by default. Adjust via the CMD in `Dockerfile.prod` |
| **Secrets management** | Never commit `local-secrets.env`. Use your cloud provider's secrets manager in production |

---

## LLM Providers

| Provider | API Key Env Var | Default Model | Notes |
|----------|-----------------|---------------|-------|
| **OpenAI** | `ARTISAN_OPENAI_API_KEY` | `gpt-4o` | Best all-around quality |
| **Anthropic** | `ANTHROPIC_API_KEY` | `claude-sonnet-4-5` | Excellent for complex instructions |
| **Mistral** | `MISTRAL_API_KEY` | `mistral-large-latest` | Fast, good quality |
| **Ollama** | *(none -- local)* | `llama3.2` | Free, runs on your machine |
| **HuggingFace** | `HUGGINGFACE_API_KEY` | `Mistral-7B-Instruct` | Inference API |

Switch providers by changing env vars -- no code changes required:

```bash
LLM_PROVIDER=anthropic
LLM_MODEL=claude-sonnet-4-5
ANTHROPIC_API_KEY=sk-ant-your-key
```

---

## AI Features

### AI Project Conversation

The standout feature: an interactive AI conversation that guides the artisan through project definition across 4 key dimensions that directly impact the manufacturing sequence: **scope, specifications, materials, and installation**. The AI asks clarifying questions in French, skipping anything already covered by the initial description. A metro-line stepper shows progress through three stages: **Description > Conversation IA > Finalisation**.

Once finalized, the user is redirected to the gamme page where they can generate the machining sequence.

Access via **Projects > Create with AI**.

### AI Gamme Generation

From any project's gamme page, generate AI-powered step-by-step work instructions. Each step includes:
- Tools and consumables needed (cross-referenced with your stock)
- Duration estimate
- Quote category (`conception`, `fabrication`, or `installation`) for automatic grouping on the quote
- Cost calculation (based on your hourly rate)

### AI Feuille de Débit (Cut List) Parsing

Upload one or more cut lists per project (PDF, image, or Excel). Each feuille is individually parsed by the AI, which extracts structured piece data: name, wood essence, dimensions (L × l × ép in mm), and quantity. The AI normalizes essence names against the user's existing stock when possible.

After parsing, a first-fit-decreasing bin-packing algorithm matches pieces against the user's wood stock. Results are displayed per-feuille with a quantity column, showing which pieces are available ("En stock") and which need purchasing ("À acheter"). An overall summary at the bottom shows total stock usage and remaining offcuts per board.

Multiple feuilles can be analyzed in parallel. Artisans typically have separate sheets for different materials (e.g., agglo vs. massif).

### AI Stock Generation

Generate a profession-specific stock template from your stock page. The app first checks for pre-generated templates in the `default_stock` table; if none exist for your profession, it falls back to real-time AI generation using a **6-step multi-call pipeline**:

| Step | Phase | What it does | Tokens |
|------|-------|-------------|--------|
| 1 | Tools | Flat list of 50-70 real tools a pro actually owns | 4000 |
| 2 | Tools | Categorize into categories/subcategories | 3000 |
| 3 | Tools | Enrich with specs | 6000 |
| 4 | Consumables | Flat list of 60-100 consumables | 5000 |
| 5 | Consumables | Categorize into categories/subcategories | 3000 |
| 6 | Consumables | Enrich with specs | 6000 |

Each step is a separate LLM call, keeping output small enough to avoid token truncation. Steps 2 & 5 validate that no items were lost during categorization. Steps 3 & 6 use `SPEC_FIELDS` from `stock_service.py` to extract structured specs.

The endpoint streams **Server-Sent Events (SSE)** so the frontend displays a real-time progress bar with step descriptions (e.g., "Etape 3/6 -- Enrichissement des outils...").

Pre-generated templates are created offline via `tools/generate_default_stocks.py` and stored in the `default_stock` table for instant loading (~1s vs ~3min for AI fallback).

### AI Shop Pattern Discovery

When adding a preferred shop, click the detect button next to the search pattern field. The AI analyzes the shop's website and discovers the correct search URL structure.

---

## Stock Specs (Chutes)

Chute (wood scrap) stock items support structured dimension specs via the `specs` JSON column:

- **`specs`** -- JSON dict with dimension values: `{"longueur": "2400", "largeur": "200", "epaisseur": "27"}`
- Used by the feuille de débit bin-packing algorithm to match cut pieces against available stock
- Displayed as editable fields on chute stock cards

### Inline editing UX

- Click the pencil icon to enter edit mode with individual spec inputs
- Changes are local until explicitly saved via Enter key or "Modifier" button
- Escape cancels and restores original values
- Quantity updates are debounced (500ms) for rapid arrow-key adjustments

---

## Quote System (Devis)

Quotes comply with French legal requirements:

- Unique sequential number (format: `DEV-{user_id}-{year}-{sequence}`)
- Supplier info (name, address, SIREN, logo)
- Client info (name, address)
- Grouped line items: steps are aggregated by category (Analyse et conception, Fabrication en atelier, Installation sur site) with summed hours
- Optional consumables line (forfait) and travel/distance line
- Optional deposit (acompte) with configurable percentage
- Subtotal HT, TVA at 20%, Total TTC
- Discount support (percentage-based)
- Signature space ("Bon pour accord") at the bottom of the PDF
- Status workflow: `draft` -> `sent` -> `accepted` / `rejected` / `expired`
- Clean PDF export via WeasyPrint (light gray/black palette)

### Setup

Configure billing info in your profile: professional address, SIREN number, hourly rate, and optionally upload your company logo.

---

## Project Structure

```
artisans-manager/
├── app/
│   ├── __init__.py                  # Flask app factory
│   ├── constants.py                 # Supported professions list
│   ├── api/                         # Route blueprints
│   │   ├── admin.py                 #   Admin invitation management
│   │   ├── auth.py                  #   Authentication (with invitation gating)
│   │   ├── clients.py               #   Client CRUD + search API
│   │   ├── planning.py              #   Calendar views + JSON events
│   │   ├── profile.py               #   Profile, shops, danger zone
│   │   ├── projects.py              #   Projects + AI conversation
│   │   ├── quotes.py                #   Quotes + PDF download
│   │   └── stock.py                 #   Stock CRUD + SSE template generation
│   ├── config/
│   │   └── project_conversation_guide.yaml
│   ├── models/                      # SQLAlchemy models
│   │   ├── user.py                  #   User + billing + shop config + is_admin
│   │   ├── invitation.py            #   Invitation codes for gated registration
│   │   ├── client.py                #   Client directory
│   │   ├── project.py               #   Projects
│   │   ├── step.py                  #   Machining steps (with quote category + planned dates)
│   │   ├── stock.py                 #   Stock inventory (item_name, specs for chutes)
│   │   ├── default_stock.py         #   Pre-generated stock templates
│   │   ├── feuille_debit.py         #   Cut lists (multiple per project)
│   │   ├── conversation.py          #   AI conversation state persistence
│   │   ├── quote.py                 #   Quotes (devis, with deposit support)
│   │   ├── quote_line.py            #   Quote line items
│   │   └── shop.py                  #   Known shops database
│   ├── services/                    # Business logic
│   │   ├── ai_service.py            #   Multi-provider LLM client + 6-step stock generation
│   │   ├── auth_service.py          #   Registration, authentication
│   │   ├── client_service.py        #   Client operations
│   │   ├── conversation_service.py  #   AI project conversations
│   │   ├── feuille_debit_service.py #   Cut list AI parsing + stock matching
│   │   ├── gamme_service.py         #   Machining sequence management
│   │   ├── pdf_service.py           #   Quote PDF generation
│   │   ├── planning_service.py      #   Calendar & scheduling
│   │   ├── pricing_service.py       #   Cost calculations
│   │   ├── project_service.py       #   Project operations + stock alerts
│   │   ├── quote_service.py         #   Quote operations
│   │   ├── stock_service.py         #   Stock operations + SPEC_FIELDS + format_specs_suffix
│   │   └── distance_service.py      #   Travel distance calculations (OSM API)
│   ├── templates/                   # Jinja2 templates (Tailwind CSS + Alpine.js)
│   │   ├── base.html
│   │   ├── components/nav.html
│   │   ├── admin/
│   │   ├── auth/
│   │   ├── clients/
│   │   ├── planning/
│   │   ├── profile/
│   │   ├── projects/
│   │   ├── quotes/
│   │   └── stock/
│   ├── utils/
│   │   ├── decorators.py            # login_required_api, owns_project, admin_required
│   │   ├── shop_helper.py           # Shop search URL builder
│   │   └── validators.py            # Input validation
│   └── static/
│       └── js/alpine.min.js
├── migrations/                      # Alembic migration history
├── tools/
│   ├── test_llm.py                  # LLM configuration test
│   ├── generate_default_stocks.py   # Batch 6-step stock template generation
│   └── generate_first_3_professions.py
├── config.py                        # Dev / Prod / Testing configs
├── run.py                           # Development server entry point
├── wsgi.py                          # Production WSGI entry point
├── test_app.py                      # Integration test script
├── Dockerfile                       # Development image
├── Dockerfile.prod                  # Production image (gunicorn, non-root)
├── docker-compose.yml               # Development compose
├── requirements.txt
└── .env.example
```

---

## Environment Variables

| Variable | Required | Default | Description |
|----------|----------|---------|-------------|
| `FLASK_ENV` | No | `development` | `development` or `production` |
| `REGISTRATION_OPEN` | No | `false` | Set to `true` to allow registration without invitation code |
| `SECRET_KEY` | Yes | -- | Flask secret key for sessions |
| `DATABASE_URL` | No | `sqlite:///artisans_dev.db` | Database connection string |
| `LLM_PROVIDER` | No | `anthropic` | LLM provider (`openai`, `anthropic`, `mistral`, `ollama`, `huggingface`) |
| `LLM_MODEL` | No | Provider default | Model name |
| `LLM_TEMPERATURE` | No | `0.7` | Response randomness (0.0-2.0) |
| `LLM_MAX_TOKENS` | No | `4000` | Max response length |
| `ARTISAN_OPENAI_API_KEY` | For OpenAI | -- | OpenAI API key |
| `ANTHROPIC_API_KEY` | For Anthropic | -- | Anthropic API key |
| `MISTRAL_API_KEY` | For Mistral | -- | Mistral API key |
| `HUGGINGFACE_API_KEY` | For HuggingFace | -- | HuggingFace API key |

---

## Tech Stack

| Layer | Technology |
|-------|------------|
| **Backend** | Python 3.12, Flask 3.0.3 |
| **ORM** | Flask-SQLAlchemy 3.1.1 |
| **Migrations** | Flask-Migrate (Alembic) |
| **Auth** | Flask-Login |
| **CSRF** | Flask-WTF |
| **PDF** | WeasyPrint 60.2 |
| **Frontend** | Tailwind CSS (CDN) + Alpine.js |
| **Database** | SQLite (dev) / PostgreSQL (prod) |
| **Server** | Flask dev server (dev) / Gunicorn (prod) |
| **Container** | Docker + Docker Compose |
| **AI** | OpenAI, Anthropic, Mistral, Ollama, HuggingFace |

---

## Docker Commands

```bash
# Development
docker-compose up              # Start with logs
docker-compose up -d           # Start detached
docker-compose up --build      # Rebuild and start
docker-compose logs -f         # Follow logs
docker-compose down            # Stop

# Production
docker build -f Dockerfile.prod -t artisans-manager:prod .
docker run -p 8000:8000 --env-file prod.env artisans-manager:prod
```

---

## Security

- Passwords hashed with werkzeug (PBKDF2)
- CSRF protection on all forms (Flask-WTF)
- Security headers: `X-Content-Type-Options`, `X-Frame-Options: DENY`, `X-XSS-Protection`, HSTS (prod), CSP
- Secure session cookies: `HTTPOnly`, `SameSite=Lax`, `Secure` (prod)
- All routes require authentication (except login/register)
- Invitation-only registration (configurable via `REGISTRATION_OPEN`)
- Admin-only routes protected by `@admin_required` decorator
- Resource ownership verification on every request
- Non-root container user in production
- Max upload size: 16 MB
