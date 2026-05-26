# GeoMAS — Multi-Agent Geolocation System

A pure Python async multi-agent geolocation framework built with security-by-design principles, replacing LLM-driven orchestration (OpenClaw) with deterministic Python control.

## Motivation

This project was originally built on the OpenClaw multi-agent framework, where an LLM "manager" agent coordinated specialist geolocation agents via natural language. While functional, this architecture introduced significant security and operational risks:

- **Uncontrolled token consumption** — LLM-to-LLM orchestration caused runaway token bloat
- **Non-deterministic behaviour** — the manager agent could retry, re-spawn, or loop unpredictably
- **No access control** — agents had unrestricted access to APIs with no budget enforcement
- **Sensitive data exposure** — images passed through LLM conversations without controls
- **No audit trail** — no structured logging of what each agent did or returned

GeoMAS rewrites this from scratch following security-first principles.

## Security-by-Design Principles

This system is designed in alignment with the **Australian Government Information Security Manual (ISM)** and **OWASP AI Security and Privacy Guidelines**, applying the following principles:

### 1. Least Privilege

- Each agent can only perform its specific function (e.g., StreetCLIP only calls localhost:8002)
- No agent has access to other agents' API keys or credentials
- Agents receive a read-only `RunContext` — they cannot modify pipeline state

### 2. Deterministic Control Flow

- All orchestration, routing, voting, and report synthesis is Python code — no LLM decisions
- Single-round dispatch: each agent runs exactly once per image, no retries at orchestrator level
- No agent can spawn other agents or escalate its own privileges

### 3. Budget and Rate Controls

- Hard per-run budget cap ($0.50 default) prevents runaway API costs
- Per-agent rate limiting (sliding window) prevents abuse
- Local inference agents (cost $0) are exempt from budget enforcement
- Cost tracking recorded per run for audit

### 4. Input Validation

- Image uploads validated for type (JPEG/PNG/WebP only) and size (20MB max)
- All agent outputs validated against Pydantic schemas before entering the pipeline
- Invalid agent responses are replaced with structured error reports — never passed downstream raw

### 5. Secrets Management

- All API keys loaded from `.env` file — never hardcoded in source
- Missing keys cause graceful agent skip (not crash)
- Keys are not logged, displayed, or passed between agents

### 6. Audit and Traceability

- Every run saves structured JSON logs to `runs/run_{timestamp}/`
- Each agent's full input/output is recorded
- Final report includes cost, latency, and status for every agent
- Real-time logging visible in terminal during execution

### 7. Data Minimisation

- Images uploaded for Google Lens search are deleted from hosting immediately after use
- Temporary files cleaned up after pipeline completion
- No persistent storage of user images beyond the run session

### 8. Failure Isolation

- Agent failures are contained — one agent crashing cannot affect others
- All exceptions caught at orchestrator level with structured error reports
- No retry escalation that could amplify failures

## Architecture

```
geomass/
  app.py                         # FastAPI entry point (POST /analyze)
  streamlit_app.py               # Interactive Streamlit UI
  core/
    schemas.py                   # Pydantic v2 data models
    registry.py                  # Agent plugin registry (BaseAgent ABC)
    orchestrator.py              # Async parallel dispatch engine
    voting.py                    # Deterministic weighted voting
    report_builder.py            # Final report synthesis
    cost_guard.py                # Budget and rate limiting
    scene_detector.py            # Indoor/outdoor classification
    context.py                   # RunContext factory
    config.py                    # Environment loading
  agents/
    streetclip_agent.py          # Local StreetCLIP (GPU inference)
    openai_vision_agent.py       # GPT-4o structured geolocation
    gemini_vision_agent.py       # Gemini 2.5 Pro/Flash with fallback
    indoor_forensics_agent.py    # Structured indoor cue extraction
    google_lens_agent.py         # SerpApi Google Lens visual matching
    faiss_retrieval_agent.py     # Vector similarity search (stub)
    siglip_geo_agent.py          # Fine-tuned SigLIP classifier (stub)
    image_search_agent.py        # Web search OSINT (stub)
    google_web_detection_agent.py # Cloud Vision Web Detection (stub)
  configs/
    agents.yaml                  # Agent weights, timeouts, parameters
```

## How It Works

1. **Image Upload** — User submits an image via Streamlit UI or FastAPI endpoint
2. **Scene Detection** — StreetCLIP classifies indoor/outdoor to adjust agent weights
3. **Parallel Dispatch** — All enabled agents run concurrently (asyncio + semaphore)
4. **Structured Output** — Each agent returns a Pydantic-validated `AgentReport`
5. **Deterministic Voting** — Python tallies weighted votes by location, scene-type aware
6. **Report Synthesis** — Final report with best guess, alternatives, evidence inventory
7. **Visual Matches** — Google Lens results displayed in sidebar with thumbnails

## Active Agents

| Agent | Type | Model | Status |
|-------|------|-------|--------|
| StreetCLIP | Local GPU | Custom CLIP | ✅ Active |
| OpenAI Vision | Cloud VLM | GPT-4o | ✅ Active |
| Gemini Vision | Cloud VLM | Gemini 2.5 Pro → Flash fallback | ✅ Active |
| Indoor Forensics | Cloud VLM | Gemini 2.5 Flash | ✅ Active |
| Google Lens | Web Search | SerpApi | ✅ Active |
| FAISS Retrieval | Local | — | ⏸️ Pending (needs index) |
| SigLIP Geo | Local GPU | — | ⏸️ Pending (needs training) |
| Image Search | Web Search | SerpApi | ⏸️ Stub |
| Google Web Detection | Cloud | Google Vision API | ⏸️ Disabled |

## Quick Start

```bash
# 1. Activate virtual environment
.venv\Scripts\activate

# 2. Run Streamlit UI (port 8510)
streamlit run geomass\streamlit_app.py --server.port 8510

# Or run FastAPI server (port 8100)
uvicorn geomass.app:app --reload --host 0.0.0.0 --port 8100
```

Or double-click the batch files:
- `run_streamlit.bat` — Opens UI at http://localhost:8510
- `run_fastapi.bat` — Opens API at http://localhost:8100/docs

## Configuration

### Environment Variables (`.env`)

```
OPENAI_API_KEY=sk-...
GEMINI_API_KEY=AIza...
SERPAPI_API_KEY=...
STREETCLIP_URL=http://127.0.0.1:8002
```

### Agent Configuration (`geomass/configs/agents.yaml`)

Each agent can be independently configured:
- `enabled` — toggle on/off without code changes
- `weight_indoor` / `weight_outdoor` — voting influence per scene type
- `timeout` — max execution time before cancellation
- `cost_per_call` — estimated cost for budget enforcement
- `rate_limit_requests` / `rate_limit_window_seconds` — API rate controls

## Cost Per Run

| Scenario | Estimated Cost |
|----------|---------------|
| Typical (3 cloud agents succeed) | $0.04–0.08 |
| All agents succeed | $0.05–0.10 |
| Worst case (retries) | $0.10–0.15 |
| Budget cap (hard limit) | $0.50 max |

## Testing

```bash
.venv\Scripts\python.exe -m pytest tests/ -v
```

138 unit tests covering schemas, registry, orchestrator, voting, cost guard, report builder, and agent error handling.

## Future Work

- FAISS vector retrieval (nearest-neighbour search against local image bank)
- SigLIP fine-tuned geolocation classifier (hierarchical heads: country → suburb)
- Full SerpApi image search integration
- Optuna experiment search for optimal voting weights
- Duplicate detection and train/test split management

## References

- [Australian Government Information Security Manual (ISM)](https://www.cyber.gov.au/resources-business-and-government/essential-cyber-security/ism)
- [OWASP AI Security and Privacy Guide](https://owasp.org/www-project-ai-security-and-privacy-guide/)
- [OWASP Top 10 for LLM Applications](https://owasp.org/www-project-top-10-for-large-language-model-applications/)
