FROM python:3.9.20-alpine

ENV PYTHONDONTWRITEBYTECODE=1

ENV PYTHONUNBUFFERED=1

RUN addgroup -S web && \
  adduser -S -G web web

USER web

WORKDIR /app

RUN mkdir collected_static

USER root

RUN --mount=type=cache,target=/root/.cache/pip \
  --mount=type=bind,source=requirements.txt,target=requirements.txt \
  pip install --upgrade pip && \
  pip install -r requirements.txt && \
  pip install gunicorn==20.1.0

COPY . .

USER web

ENTRYPOINT ["/app/docker-entrypoint.sh"]

CMD [ "gunicorn", "--bind", "0.0.0.0:8000", "backend.wsgi"]

EXPOSE 8000
