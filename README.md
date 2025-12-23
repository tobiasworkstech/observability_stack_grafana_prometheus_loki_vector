# Observability Stack

A complete observability stack with Grafana, Prometheus, Loki, and Vector.

## Components

| Service    | Port | Description                          |
|------------|------|--------------------------------------|
| Grafana    | 3000 | Visualization and dashboards         |
| Prometheus | 9090 | Metrics storage and querying         |
| Loki       | 3100 | Log aggregation                      |
| Vector     | 8686 | Log/metrics collection and routing   |

## Quick Start

```bash
# Start all services
docker-compose up -d

# Check status
docker-compose ps

# View logs
docker-compose logs -f
```

## Access

- **Grafana**: http://localhost:3000
  - Username: `admin`
  - Password: `admin`
  
- **Prometheus**: http://localhost:9090

- **Loki**: http://localhost:3100

## Pre-configured Datasources

Grafana comes pre-configured with:
- **Prometheus** (default) - for metrics
- **Loki** - for logs

## Vector Configuration

Vector is configured to:
1. Collect Docker container logs
2. Collect host system logs from `/var/log`
3. Accept logs via HTTP endpoint on port 8080
4. Forward all logs to Loki
5. Expose metrics for Prometheus to scrape

### Sending Logs to Vector

```bash
# Send a log entry via HTTP
curl -X POST http://localhost:8080 \
  -H "Content-Type: application/json" \
  -d '{"message": "Hello from my app", "level": "info"}'
```

## Directory Structure

```
.
├── docker-compose.yml
├── grafana/
│   └── provisioning/
│       └── datasources/
│           └── datasources.yml
├── loki/
│   └── loki-config.yml
├── prometheus/
│   └── prometheus.yml
└── vector/
    └── vector.yml
```

## Customization

### Adding Application Metrics

Edit `prometheus/prometheus.yml` to add scrape targets:

```yaml
scrape_configs:
  - job_name: "my-app"
    static_configs:
      - targets: ["my-app:8080"]
```

### Adding Log Sources

Edit `vector/vector.yml` to add new sources and transforms.

## Stopping the Stack

```bash
docker-compose down

# Remove volumes too
docker-compose down -v
```
