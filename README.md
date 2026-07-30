<div align="center">

# ⚡ NexusCore

**Enterprise NOC Platform** — Real-time network operations, AD replication monitoring, AI-powered anomaly detection, and SIEM integration.

[![License](https://img.shields.io/badge/license-MIT-brightgreen)](LICENSE)
[![Language](https://img.shields.io/badge/python-3.12%2B-blue)](https://python.org)
[![React](https://img.shields.io/badge/react-18-61DAFB?logo=react)](https://react.dev)
[![FastAPI](https://img.shields.io/badge/fastapi-latest-009688?logo=fastapi)](https://fastapi.tiangolo.com)
[![Docker](https://img.shields.io/badge/docker-compose-2496ED?logo=docker)](https://docker.com)
[![PostgreSQL](https://img.shields.io/badge/postgres-16-4169E1?logo=postgresql)](https://postgresql.org)
[![Last Commit](https://img.shields.io/github/last-commit/OneByJorah/NexusCore)](https://github.com/OneByJorah/NexusCore)

</div>

---

<p align="center">
  <img src="screenshot.png" alt="NexusCore Dashboard" width="100%">
</p>

---

## 📋 Overview

NexusCore is a production-grade **Network Operations Center (NOC)** platform built for enterprise environments. It provides real-time visibility into Active Directory replication health, NTP synchronization, DNS resolution, PBX telephony status, and helpdesk ticket metrics — all through a unified, dark-themed dashboard with AI-powered insights via Ollama/OpenAI integration.

| Capability | Description |
|---|---|
| **NOC Dashboard** | Real-time overview of all network operations in a single pane |
| **AD Replication** | Active Directory health across all domain controllers |
| **NTP/DNS/PBX** | Service health with automated alerting and status tracking |
| **Helpdesk** | Ticket metrics, SLA tracking, and status management |
| **AI Insights** | GPT / Ollama integration for intelligent anomaly detection |
| **SNMP Discovery** | Automated network device discovery and inventory |
| **SIEM** | Wazuh integration for security event monitoring |

---

## 🏗️ Architecture

```mermaid
graph TB
    subgraph Frontend
        REACT[React 18 + TypeScript]
        VITE[Vite Dev Server]
    end
    
    subgraph Backend
        API[FastAPI]
        ALEMBIC[Alembic Migrations]
        SQLA[SQLAlchemy ORM]
    end
    
    subgraph Collectors
        AD[AD/LDAP Collector]
        NTP[NTP Monitor]
        DNS[DNS Benchmark]
        PBX[PBX Monitor]
        SNMP[SNMP Exporter]
    end
    
    subgraph AI
        OLLAMA[Ollama Local]
        OPENAI[OpenAI GPT]
    end
    
    subgraph Storage
        PG[(PostgreSQL)]
        REDIS[(Redis)]
    end
    
    subgraph Monitoring
        PROM[Prometheus]
        GRAF[Grafana]
        LOKI[Loki]
        WAZUH[Wazuh SIEM]
        CSEC[CrowdSec]
    end
    
    REACT -->|HTTP/REST| API
    VITE --> REACT
    API --> SQLA
    SQLA --> PG
    API --> REDIS
    API --> AD & NTP & DNS & PBX
    SNMP --> PROM
    PROM --> GRAF
    LOKI --> GRAF
    API --- OLLAMA & OPENAI
    API --- WAZUH
    CSEC --> NGINX
    NGINX[nginx] --> REACT & API
```

---

## 🛠️ Tech Stack

**Backend** · `FastAPI` · `Python 3.12+` · `SQLAlchemy` · `Alembic` · `PostgreSQL` · `Redis`

**Frontend** · `React 18` · `TypeScript` · `Vite` · `TailwindCSS` · `Recharts`

**AI/ML** · `OpenAI GPT` · `Ollama` (local LLMs: Llama 3.2, Mistral, Phi3)

**Monitoring** · `Prometheus` · `Grafana` · `Loki` · `SNMP Exporter` · `CrowdSec` · `Wazuh`

**DevOps** · `Docker Compose` · `systemd` · `GitHub Actions CI` · `pre-commit` · `ruff`

---

## 🚀 Quick Start

```bash
git clone https://github.com/OneByJorah/NexusCore.git
cd NexusCore

cp .env.example .env          # Configure your services
docker compose up -d          # Launch everything
```

Open **http://localhost:3000** in your browser.

### Local Development

```bash
# Backend
cd backend
pip install -r requirements.txt
alembic upgrade head
uvicorn main:app --reload

# Frontend
cd frontend
npm install
npm run dev
```

---

## 🔧 Environment Variables

| Variable | Default | Description |
|---|---|---|
| `OPENAI_API_KEY` | — | OpenAI API key for AI-powered insights |
| `DATABASE_URL` | `sqlite:///./nexuscore.db` | PostgreSQL/SQLite connection string |
| `AD_DOMAIN_CONTROLLER` | — | Domain controller hostname for monitoring |
| `NTP_SERVERS` | — | Comma-separated NTP servers to check |
| `DNS_SERVERS` | — | Comma-separated DNS servers to check |
| `PBX_HOST` | — | PBX server hostname for status monitoring |

See `.env.example` for all available options.

---

## 📁 Project Structure

```
NexusCore/
├── backend/                    # FastAPI application
│   ├── main.py                 # Entry point
│   ├── routers/                # API endpoint modules
│   ├── services/               # Business logic & collectors
│   └── models/                 # SQLAlchemy database models
├── frontend/                   # React SPA dashboard
│   ├── src/
│   │   ├── pages/              # Dashboard pages
│   │   └── components/         # Reusable React components
│   └── package.json
├── alembic/                    # Database migrations
├── monitoring/                 # Prometheus, Grafana, Loki configs
├── nginx/                      # Reverse proxy configs
├── admin-service/              # Admin utilities
├── agent/                      # Monitoring agents
├── docs/                       # Documentation & assets
├── scripts/                    # Utility scripts
├── database/                   # DB init scripts
├── systemd/                    # systemd service units
├── docker-compose.yml          # Production deployment
└── .env.example                # Configuration template
```

---

## 📡 API Endpoints

| Endpoint | Method | Description |
|---|---|---|
| `/api/dashboard` | GET | NOC dashboard overview |
| `/api/ad/replication` | GET | AD replication status |
| `/api/ntp/status` | GET | NTP synchronization health |
| `/api/dns/health` | GET | DNS resolution status |
| `/api/pbx/status` | GET | PBX service health |
| `/api/tickets` | GET | Helpdesk ticket metrics |
| `/api/ai/insights` | GET | AI-powered anomaly insights |

---

## 🤝 Contributing

Contributions welcome! Please see [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines and [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md) for community standards.

All contributions follow the [Code of Conduct](CODE_OF_CONDUCT.md).

## 🔒 Security

Found a vulnerability? Please follow our [Security Policy](SECURITY.md) and report privately to **info@jorahone.com** — do not use public issues.

---

## 📄 License

[MIT License](LICENSE) © Jhonattan L. Jimenez (OneByJorah)

---

<p align="center">
  Built with 🌴 by <a href="https://github.com/OneByJorah">OneByJorah</a> ·
  <a href="https://jorahone.com">jorahone.com</a>
</p>
