# Start Service with OTel Agent
OTEL_PYTHON_LOG_CORRELATION=true OTEL_PYTHON_LOGGING_AUTO_INSTRUMENTATION_ENABLED=true OTEL_EXPORTER_OTLP_ENDPOINT=http://host.docker.internal:4317 opentelemetry-instrument --traces_exporter otlp --metrics_exporter none --logs_exporter otlp --service_name gateway flask run --port 3001

# Enable OTel Log Instrumentation

## Mac/Linux
export OTEL_PYTHON_LOG_CORRELATION=true
export OTEL_PYTHON_LOGGING_AUTO_INSTRUMENTATION_ENABLED=true

## Windows
set OTEL_PYTHON_LOG_CORRELATION=true
set OTEL_PYTHON_LOGGING_AUTO_INSTRUMENTATION_ENABLED=true