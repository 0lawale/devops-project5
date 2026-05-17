# DevOps Project 5 — Monitoring & Alerting Stack

A production-grade observability stack built with Prometheus, Grafana, Loki and
Alertmanager — fully automated with GitHub Actions CI/CD and Slack alerting.

---

## Architecture

```
Applications & Host
        │
        ▼
┌─────────────────────────────────────────┐
│           Monitoring Stack              │
│                                         │
│  node-exporter  →  Prometheus           │
│                        │                │
│                        ▼                │
│                   Alertmanager          │
│                        │                │
│                        ▼                │
│                  Slack Alerts           │
│                                         │
│  Promtail  →  Loki  →  Grafana          │
│                        ▲                │
│               Prometheus                │
└─────────────────────────────────────────┘
```

---

## Tech Stack

| Tool | Purpose |
|------|---------|
| Prometheus | Metrics collection and alerting rules |
| Grafana | Metrics and logs visualisation |
| Alertmanager | Alert routing and Slack notifications |
| Loki | Log aggregation |
| Promtail | Log shipping agent |
| Node Exporter | Host metrics (CPU, memory, disk, network) |
| Docker Compose | Stack orchestration |
| GitHub Actions | CI/CD pipeline |

---

## CI/CD Pipeline

| Step | What it does |
|------|-------------|
| **Validate Prometheus config** | Checks prometheus.yml is valid |
| **Validate Alertmanager config** | Checks alertmanager.yml is valid |
| **Start stack** | Spins up all 6 services |
| **Health checks** | Verifies Prometheus, Grafana and Alertmanager are up |
| **Target check** | Confirms all scrape targets are healthy |
| **Tear down** | Cleans up after pipeline |

---

## Alert Rules

| Alert | Condition | Severity |
|-------|-----------|----------|
| `InstanceDown` | Target unreachable for > 1 min | Critical |
| `HighCPUUsage` | CPU > 80% for > 1 min | Warning |
| `HighMemoryUsage` | Memory > 80% for > 1 min | Warning |

---

## Running Locally

**Prerequisites:** Docker, Docker Compose

```bash
git clone https://github.com/0lawale/devops-project5.git
cd devops-project5

# Set your Slack webhook
export SLACK_WEBHOOK_URL=https://hooks.slack.com/services/xxx/yyy/zzz

docker compose up -d
```

Access the stack:
- **Prometheus:** http://localhost:9090
- **Grafana:** http://localhost:3000 (admin / devops2024)
- **Alertmanager:** http://localhost:9093

Import the Node Exporter Full dashboard in Grafana using ID `1860`.

---

## Project Highlights

- **End-to-end observability** — metrics, logs and alerting in a single stack
- **Automated config validation** — pipeline validates all config files before deployment
- **Slack alerting** — real-time notifications with resolved messages when issues clear
- **Pre-built dashboards** — Node Exporter Full dashboard imported automatically
- **Cache-aside pattern** — Grafana provisioning auto-configures Prometheus and Loki datasources on startup
- **Secret management** — Slack webhook stored as GitHub secret, never in code