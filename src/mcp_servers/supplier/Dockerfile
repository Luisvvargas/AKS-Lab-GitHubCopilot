FROM python:3.11-slim AS builder

ENV UV_SYSTEM_PYTHON=1
RUN python -m pip install --no-cache-dir uv
RUN uv pip install --system \
    azure-identity==1.27.0 \
    azure-keyvault-secrets==4.11.3 \
    anyio==4.15.1 \
    certifi==2026.10.23 \
    cryptography==50.0.2 \
    mcp==1.30.0 \
    PyJWT==2.15.1 \
    pydantic \
    structlog

FROM python:3.11-slim AS runtime

LABEL org.opencontainers.image.source="https://github.com/kinfey/AKS-Lab-GitHubCopilot"

ENV PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1

RUN useradd -m -u 10001 zava
WORKDIR /app

COPY --from=builder /usr/local/lib/python3.11/site-packages /usr/local/lib/python3.11/site-packages
COPY src /app/src

USER zava
EXPOSE 8080

CMD ["python", "-m", "src.mcp_servers.supplier.server"]