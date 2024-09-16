ARG PYTHON_VERSION=3.9.20-alpine

FROM python:${PYTHON_VERSION}

WORKDIR /app

ENV PYTHONDONTWRITEBYTECODE=1

ENV PYTHONUNBUFFERED=1

RUN --mount=type=cache,target=/root/.cache/pip \
  --mount=type=bind,source=requirements.txt,target=requirements.txt \
  pip install --upgrade pip && \
  pip install -r requirements.txt && \
  pip install gunicorn==20.1.0

COPY . .

RUN chmod 755 docker-entrypoint.sh && \
  mkdir -p /app/collected_static/ && \
  addgroup -S web && \
  adduser -S -G web web && \
  chown -R web:web /app

USER web

ENTRYPOINT ["/app/docker-entrypoint.sh"]

CMD [ "gunicorn", "--bind", "0.0.0.0:8000", "backend.wsgi"]

EXPOSE 8000
