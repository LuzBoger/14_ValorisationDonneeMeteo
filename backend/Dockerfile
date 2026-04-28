FROM dhi.io/python:3.12-dev AS builder

WORKDIR /build

COPY --from=ghcr.io/astral-sh/uv:latest /uv /uv/bin/

COPY pyproject.toml uv.lock ./
RUN /uv/bin/uv sync --frozen --no-install-project --no-dev

FROM dhi.io/python:3.12 AS prod

WORKDIR /app

COPY --from=builder --chown=1000:1000 /build/.venv /app/.venv
COPY --chown=1000:1000 . .

ENV PATH="/app/.venv/bin:$PATH" \
    PYTHONUNBUFFERED=1 \
    PYTHONDONTWRITEBYTECODE=1

USER 1000

EXPOSE 8000

ENTRYPOINT ["gunicorn"]
CMD ["--bind", "0.0.0.0:8000", "config.wsgi:application"]
