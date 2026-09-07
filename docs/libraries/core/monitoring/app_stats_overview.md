# App Stats — Overview

`ecodev_core` ships a lightweight statistics layer (`app_stats`) that lets any app **expose** its
usage data over HTTP and lets any central dashboard **ingest** that data on a schedule.

The two roles are deliberately separated:

| Role | Name | What it does |
|------|------|--------------|
| **Producer** | any `ecodev_core` app | Serves aggregated activity & project data via `/stats/*` FastAPI routes |
| **Consumer** | a central monitoring app | Polls producers on a schedule, stores results locally, and renders dashboards |

---

## How they fit together

```
  ┌─────────────────────┐        /stats/activities     ┌──────────────────────────┐
  │   Producer app A    │ ◄─────────────────────────── │                          │
  │  (e.g. cf-tool)     │                              │   Consumer / monitoring  │
  └─────────────────────┘                              │          app             │
                                                       │                          │
  ┌─────────────────────┐        /stats/projects        │  • polls on a schedule   │
  │   Producer app B    │ ◄─────────────────────────── │  • stores locally        │
  │  (e.g. myecoapps)   │                              │  • renders dashboards    │
  └─────────────────────┘                              └──────────────────────────┘
```

The consumer never queries the producers at render time — network calls happen during the
scheduled ingest only, keeping dashboards fast and resilient to producer downtime.

---

## Producer in a nutshell

Wire `get_stats_router()` into an existing FastAPI app:

```python
from ecodev_core import get_stats_router

app.include_router(get_stats_router())
```

This registers `/stats/activities`.  Optionally pass a `ProjectStatsAdapter` to also expose
`/stats/projects`.  Authentication is handled via a per-environment `api_key` — no JWT needed.

→ See the [Producer guide](producer_guide.md) for the full setup.

---

## Consumer in a nutshell

Import the consumer submodule, store producer configs (URLs + API keys), and run the ingest
command on a schedule:

```python
from ecodev_core.app_stats.consumer import StatsApiClient, upsert_remote_activities

# nightly via Ofelia / Typer command
run_ingest(granularity=HOUR_GRAIN)
```

Ingested rows land in local `RemoteActivity` and `RemoteProject` tables and are read back
with `get_remote_activities` / `get_remote_projects`.

→ See the [Consumer guide](consumer_guide.md) for the full setup.

---

!!! tip
    Both guides share the same `ecodev_core.app_stats.constants` module for grain constants
    (`HOUR_GRAIN`, `MONTH_GRAIN`). Import them from there to avoid typos.
