ARG NODE_VERSION=20.17-alpine

FROM node:${NODE_VERSION} AS build

WORKDIR /app

RUN --mount=type=cache,target=/root/.npm \
  --mount=type=bind,source=package.json,target=package.json \
  --mount=type=bind,source=package-lock.json,target=package-lock.json \
  npm ci

COPY . .

RUN npm run build


FROM node:${NODE_VERSION} AS production

COPY --from=build /app/build /app/build

RUN addgroup -S web && \
  adduser -S -G web web &&\
  chown -R web:web /app

USER web
