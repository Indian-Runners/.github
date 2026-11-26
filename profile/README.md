# NYX

**A self-hosted data platform for organizations that collect data in the field.**

NYX bundles the whole chain — data capture, ingestion, storage, analytics and paper
reporting — into one Docker Compose stack you run on-premise or in your own cloud account.
The infrastructure (message broker, databases, reverse proxy, pipelines) is already wired
together, so you work on functionality instead of plumbing.

[![Elasticsearch](https://img.shields.io/badge/elasticsearch-005571?style=flat-square&logo=elasticsearch&logoColor=white)](https://www.elastic.co/)
[![PostgreSQL](https://img.shields.io/badge/postgresql-4169E1?style=flat-square&logo=postgresql&logoColor=white)](https://www.postgresql.org/)
[![Docker Compose](https://img.shields.io/badge/docker%20compose-2496ED?style=flat-square&logo=docker&logoColor=white)](https://docs.docker.com/compose/)
[![Vue.js](https://img.shields.io/badge/vue.js-42b883?style=flat-square&logo=vuedotjs&logoColor=white)](https://vuejs.org/)

![NYX overview](https://raw.githubusercontent.com/Indian-Runners/nyx/master/medias/overview.gif)

## What NYX does

- **Normalize incoming data.** Apache Camel workers clean, reshape and load data
  automatically, so what lands in the database is ready to query.
- **Answer questions without building infrastructure.** Kibana, Grafana, Jupyter notebooks
  and Vega visualizations all read the same data.
- **Ship paper reports on a schedule.** Jasper-based runners generate PDF/XLS reports on a
  timer and store them for download.
- **Know when something breaks.** Container monitoring, metrics and centralized logs keep
  watch over the platform.
- **Everything talks through a broker.** ActiveMQ carries ingestion, report and status
  messages between components, so pieces can be replaced independently.

## Analytics, on the data you already have

![Jupyter notebooks querying the NYX data](https://raw.githubusercontent.com/Indian-Runners/nyx/master/medias/overview-apps.gif)

Kibana and Grafana dashboards, Jupyter notebooks and Vega specs are all first-class clients
of the same Elasticsearch and PostgreSQL stores — no extract, no second copy of the data.

## Components hosted here

| Repository | Role | Stack |
| --- | --- | --- |
| [nyx](https://github.com/Indian-Runners/nyx) | The platform itself: Docker Compose stack, OpenResty routing, service configuration, backup scripts | Compose, OpenResty |
| [nyx_rest](https://github.com/Indian-Runners/nyx_rest) | REST API behind the console and the integrations, Swagger UI at `/api/doc/` | Python, gunicorn |
| [nyx_ui](https://github.com/Indian-Runners/nyx_ui) | Web interface, served by OpenResty at `/` — screens, panels, query builder | Vue 3, Element Plus |
| [nyx_containers](https://github.com/Indian-Runners/nyx_containers) | Container definitions used to ship, clean and process data: JupyterLab, Lambda workers, report runner | Python, Docker |
| [nyx_ws_server](https://github.com/Indian-Runners/nyx_ws_server) | WebSocket server bridged to ActiveMQ, pushes live module status to the UI | Node.js |
| [nyx_logs](https://github.com/Indian-Runners/nyx_logs) | Log and file browser of the console: directory listing, filtering, in-place file reading | Vue 3, TypeScript, Vite |
| [nyx_jasper_creator](https://github.com/Indian-Runners/nyx_jasper_creator) | Scheduled PDF/XLS report generation driven by ActiveMQ messages, with a REST API for template upload | Java, JasperReports |

The platform is not a monorepo: each component is released, tested and versioned on its own, and
the stack pins the image tags. That is what lets one worker be replaced without rebuilding the
whole platform.

## Getting started

```shell
git clone https://github.com/Indian-Runners/nyx.git
cd nyx/nyx/docker-compose
cp .env.example .env          # set PG_PASSWORD and AMQC_PASSWORD
docker compose up -d
```

Full prerequisites, host directories and the TLS key that are not in git are documented in
the [main README](https://github.com/Indian-Runners/nyx#getting-started).

## Status

NYX is an actively developed, production-deployed platform. Each repository ships its own
test suite and CI; badges at the top of each README reflect the current state of that
component.

## Contributing

Issues and pull requests are welcome on the repository concerned. For anything spanning
several components, open the discussion on [nyx](https://github.com/Indian-Runners/nyx/issues).
