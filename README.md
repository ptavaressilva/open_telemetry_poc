# Open Telemetry PoC

Example of Open Telemetry usage (metrics, traces and logs) with Flask, Docker and Prometheus.

## Requirements

To run this example you need:

- Docker
- uv

## Setup

```bash
uv venv
source .venv/bin/activate
docker run --rm \
    -v /YOUR_PATH_TO/otel-getting-started/prometheus/prometheus.yml:/prometheus/prometheus.yml \
    -p 9090:9090 prom/prometheus \
    -d \
    --enable-feature=otlp-write-receive
docker run -p 4317:4317 \                                             
    -v /YOUR_PATH_TO/otel-getting-started/tmp/otel-collector-config.yaml:/etc/otel-collector-config.yaml \
    -d \
    otel/opentelemetry-collector:latest \
    --config=/etc/otel-collector-config.yaml
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

You can reach Prometheus' UI at `http://localhost:9090`.
