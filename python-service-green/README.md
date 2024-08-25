# Start Service with OTel Agent
OTEL_EXPORTER_OTLP_ENDPOINT=http://host.docker.internal:4317  opentelemetry-instrument --traces_exporter otlp --metrics_exporter none --logs_exporter none --service_name service-green flask run --port 3010

# Enable OTel Log Instrumentation

## Mac/Linux
export OTEL_PYTHON_LOG_CORRELATION=true
export OTEL_PYTHON_LOGGING_AUTO_INSTRUMENTATION_ENABLED=true

## Windows
set OTEL_PYTHON_LOG_CORRELATION=true
set OTEL_PYTHON_LOGGING_AUTO_INSTRUMENTATION_ENABLED=true