# Open Telemetry PoC

Example of Open Telemetry usage (metrics, traces and logs) with Flask, Docker, Prometheus (with Prometheus OTLP Receiver) and Grafana.

## Requirements

To run this example you need:

- Docker
- uv

## Setup

Start the Prometheus collector with:

```bash
docker run \
    --rm \
    -d \
    -v /YOUR_REPO_PATH/otel-getting-started/prometheus/prometheus.yml:/prometheus/prometheus.yml \
    -p 9090:9090 \
    --name=prometheus \
    prom/prometheus \
    --enable-feature=otlp-write-receive
```

To start Grafana:

```bash
docker run \
    --rm \
    -d \
    -p 3000:3000 \
    --name=grafana \
    grafana/grafana-enterprise
```

## Running the app

To run the app use the following commands:

```bash
uv venv
source .venv/bin/activate
export OTEL_PYTHON_LOGGING_AUTO_INSTRUMENTATION_ENABLED=true
opentelemetry-instrument --logs_exporter otlp flask run -p 8080
```

Once the app is running, go to `http://localhost:8080/rolldice`. Each time you refresh the page a dice roll will be made and logs, traces and metrics will be generated.

You can reach Prometheus' UI at `http://localhost:9090` and Grafana's UI at `http://localhost:3000` (admin/admin).
