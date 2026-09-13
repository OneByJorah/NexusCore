<div align="center">

![NexusCore](docs/assets/banner.svg)

# NexusCore

**Enterprise NOC platform — unified monitoring for AD replication, NTP, DNS, PBX, and helpdesk, with AI-powered anomaly detection and SIEM integration.**

[![GitHub release](https://img.shields.io/github/v/release/OneByJorah/NexusCore?color=00b4d8&label=release&logo=github)](https://github.com/OneByJorah/NexusCore/releases)
[![PyPI version](https://img.shields.io/pypi/v/j1-noc-platform-backend?color=00b4d8&label=pip&logo=pypi)](https://pypi.org/project/j1-noc-platform-backend/)
[![Docker Pulls](https://img.shields.io/docker/pulls/ghcr.io/onebyjorah/nexuscore?color=00b4d8&label=docker&logo=docker)](https://ghcr.io/onebyjorah/nexuscore)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?color=FFB300&logo=open-source-initiative&logoColor=FFB300)](https://opensource.org/licenses/MIT)
[![Python 3.12+](https://img.shields.io/badge/Python-3.12+-3776AB?style=flat-square&logo=python&logoColor=white)](https://www.python.org/)
[![FastAPI](https://img.shields.io/badge/FastAPI-0.139-009688?style=flat-square&logo=fastapi&logoColor=white)](https://fastapi.tiangolo.com/)
[![React 18](https://img.shields.io/badge/React-18-61DAFB?style=flat-square&logo=react&logoColor=white)](https://react.dev/)
[![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat-square&logo=docker&logoColor=white)](https://www.docker.com/)

</div>

![NexusCore dashboard](docs/assets/screenshot.png)

## What This Is

NexusCore consolidates the core infrastructure signals an enterprise NOC watches — Active Directory replication, NTP synchronization, DNS resolution, PBX telephony health, and helpdesk tickets — into one dark-themed operations dashboard with a navy-and-cyan visual identity. It adds AI-assisted insights via a local Ollama model, Wazuh SIEM status, and a Prometheus/Grafana/Loki monitoring path.

It is built for infrastructure teams that need a single pane of glass across directory, time, name, voice, and support services, deployed entirely on-premises with Docker Compose.

## Quick Start

```bash
git clone https://github.com/OneByJorah/NexusCore.git
cd NexusCore
cp .env.example .env      # set SECRET_KEY, DATABASE_URL, POSTGRES_PASSWORD, REDIS_PASSWORD
docker compose up -d
```

Open **http://localhost:5173** (nginx). Grafana is on **http://localhost:3000**, Prometheus on **http://localhost:9090**.

> [!WARNING]
> Every secret in `.env.example` is a placeholder. Replace `SECRET_KEY`, `POSTGRES_PASSWORD`, and `REDIS_PASSWORD` before deploying, and do not expose the stack to untrusted networks until they are set.

## Installation

### Docker (recommended)

```bash
git clone https://github.com/OneByJorah/NexusCore.git
cd NexusCore
cp .env.example .env
docker compose up -d
```

### pip

```bash
pip install j1-noc-platform-backend
```

### From Source

```bash
git clone https://github.com/OneByJorah/NexusCore.git
cd NexusCore
cd backend && pip install -e .
```

## Features

- **NOC dashboard** — real-time overview of all monitored services in one pane.
- **AD replication monitoring** — domain controller status with a force-replication action.
- **NTP & DNS health** — Chrony sync status, NTP client list, and DNS benchmarking.
- **PBX telephony** — Mitel PBX service health plus SNMP walk results.
- **Helpdesk metrics** — osTicket ticket listing and creation.
- **AI insights** — Ollama (and OpenAI-compatible) endpoints for anomaly triage.
- **Wazuh SIEM** — agent, alert, and overview queries from the dashboard.
- **Admin & onboarding** — role, user, tab, and encrypted-settings management plus a first-run setup API.
- **Observability stack** — Prometheus metrics, Grafana dashboards, Loki logs, CrowdSec, and SNMP exporter.
- **Migrations** — schema managed exclusively through Alembic.

## Architecture

```
React (Vanilla JS SPA) ──HTTP/REST──▶ FastAPI /api ──▶ PostgreSQL
                                       │
                                       ├──▶ Redis (cache)
                                       ├──▶ Collectors: AD/LDAP · NTP · DNS · PBX
                                       ├──▶ Ollama / OpenAI (AI insights)
                                       └──▶ Wazuh SIEM · osTicket

nginx :5173/:8443 ──▶ static dashboard + /api proxy
Prometheus ◀── /metrics   ·   Grafana ◀── Prometheus + Loki
```

> [!NOTE]
> The dashboard UI served is the standalone HTML/JS app in `frontend/index.html`. The `frontend/src/` React SPA is present but not yet wired into the Vite entry point.

## Tech Stack

- **Backend** — FastAPI, Python 3.12+, SQLAlchemy, Alembic, PostgreSQL 16, Redis
- **Frontend** — React 18, TypeScript, Vite, TailwindCSS, Recharts (standalone `index.html` is the served UI)
- **AI/ML** — Ollama local LLMs, OpenAI-compatible endpoints
- **Monitoring** — Prometheus, Grafana, Loki, SNMP Exporter, CrowdSec, Wazuh
- **DevOps** — Docker Compose, systemd, GitHub Actions, pre-commit, ruff

## Configuration

Copy `.env.example` to `.env`.

| Variable | Default | Description |
|----------|---------|-------------|
| `SECRET_KEY` | — | JWT signing key (**required**) |
| `DATABASE_URL` | `postgresql://jnop:***@postgres:5432/jnop` | PostgreSQL connection string |
| `REDIS_URL` | `redis://redis:6379` | Redis cache URL |
| `REDIS_PASSWORD` | — | Redis password |
| `POSTGRES_USER` / `POSTGRES_PASSWORD` / `POSTGRES_DB` | `jnop` / — / `jnop` | Postgres credentials |
| `BACKEND_CORS_ORIGINS` | `http://localhost:5173` | Allowed CORS origins |
| `GRAFANA_ADMIN_PASSWORD` | — | Grafana admin password |
| `MITEL_SNMP_HOST` / `MITEL_SNMP_COMMUNITY` | `localhost` / `public` | PBX SNMP target |
| `OSTICKET_BASE_URL` / `OSTICKET_API_KEY` | — | osTicket helpdesk integration |
| `LDAP_URL` / `LDAP_DOMAIN` / `LDAP_BIND_DN` / `LDAP_BIND_PASSWORD` | — | LDAP/AD binding |
| `CHRONY_SERVER` | `localhost` | NTP/Chrony server to monitor |
| `WAZUH_API_URL` / `WAZUH_USERNAME` / `WAZUH_PASSWORD` | — | Wazuh SIEM connection |
| `OLLAMA_URL` / `OLLAMA_HOST` | `http://localhost:11434` | Local LLM endpoint |
| `TELEGRAM_BOT_TOKEN` / `TEAMS_WEBHOOK` | — | Notification channels |

## API Endpoints

All application routes are served under `/api` by FastAPI (interactive docs at `/api/docs`).

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/api/dashboard/overview` | GET | NOC dashboard overview metrics |
| `/api/system/overview` | GET | Live service-health probes |
| `/api/dc_status` | GET | AD domain controller replication status |
| `/api/dc/forcerepl` | POST | Force AD replication |
| `/api/ntp_status` | GET | NTP client synchronization health |
| `/api/ntp_clients` | GET | NTP client list |
| `/api/pbx/status` | GET | PBX service health |
| `/api/pbx/snmp/walk` | GET | Mitel SNMP walk results |
| `/api/helpdesk/tickets` | GET/POST | Helpdesk ticket metrics (osTicket) |
| `/api/wazuh/status` | GET | Wazuh SIEM connection status |
| `/api/wazuh/alerts` | GET | Recent Wazuh alerts |
| `/api/ollama/chat` | POST | AI insights via Ollama |
| `/api/ollama/status` | GET | Ollama connectivity/models |
| `/api/admin/users` | GET/POST | User administration (admin role) |
| `/api/admin/roles` | GET/POST | Role administration |
| `/api/admin/settings` | GET/PUT | Encrypted settings management |
| `/api/auth/login` | POST | Obtain a JWT |
| `/metrics` | GET | Prometheus metrics |
| `/healthz` | GET | Liveness probe |

## Package Badges

[![GitHub release](https://img.shields.io/github/v/release/OneByJorah/NexusCore?color=00b4d8&label=release&logo=github)](https://github.com/OneByJorah/NexusCore/releases)
[![PyPI version](https://img.shields.io/pypi/v/j1-noc-platform-backend?color=00b4d8&label=pip&logo=pypi)](https://pypi.org/project/j1-noc-platform-backend/)
[![Docker Pulls](https://img.shields.io/docker/pulls/ghcr.io/onebyjorah/nexuscore?color=00b4d8&label=docker&logo=docker)](https://ghcr.io/onebyjorah/nexuscore)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?color=FFB300&logo=open-source-initiative&logoColor=FFB300)](https://opensource.org/licenses/MIT)

## Testing

```bash
cd backend && python -m pytest tests -q
cd frontend && npm install && npm run build
```

See [TESTING.md](TESTING.md) for details.

## Use Cases

1. **Enterprise NOC** — a single pane across directory, time, name, voice, and support services.
2. **Infrastructure teams** — correlate AD replication and NTP health before incidents escalate.
3. **Security operations** — surface Wazuh SIEM status alongside AI anomaly triage.

## Screenshots

| NOC Dashboard | AD Replication | AI Insights |
|---|---|---|
| ![NOC](docs/screenshots/noc-dashboard.png) | ![Replication](docs/screenshots/dc-replication.png) | ![AI](docs/screenshots/ai-insights.png) |

| NTP Monitor | Wazuh SIEM | Helpdesk |
|---|---|---|
| ![NTP](docs/screenshots/ntp-monitor.png) | ![Wazuh](docs/screenshots/wazuh-siem.png) | ![Helpdesk](docs/screenshots/helpdesk.png) |

More captures live in [`docs/screenshots/`](docs/screenshots/).

## Contributing

Contributions are welcome — see [CONTRIBUTING.md](CONTRIBUTING.md). [Open an issue](https://github.com/OneByJorah/NexusCore/issues) to report a bug or request a feature.

## License

MIT — see [LICENSE](LICENSE).

## Connect

- [jorahone.com](https://jorahone.com)
- [GitHub Org](https://github.com/OneByJorah)
- [info@jorahone.com](mailto:info@jorahone.com)
