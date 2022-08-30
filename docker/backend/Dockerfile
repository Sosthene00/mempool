FROM node:16.16.0-buster-slim AS builder

ARG commitHash
ENV DOCKER_COMMIT_HASH=${commitHash}

RUN apt-get update
RUN apt-get install -y build-essential python3 pkg-config

WORKDIR /build
COPY . .
WORKDIR /build/backend

RUN npm install --omit=dev --omit=optional
RUN npm run build

FROM node:16.16.0-buster-slim

WORKDIR /backend

COPY --from=builder /build/backend .
COPY --from=builder /build/docker/backend/* ./

RUN chmod +x ./start.sh
RUN chmod +x ./wait-for-it.sh

RUN chown -R 1000:1000 /backend && chmod -R 755 /backend

USER 1000

EXPOSE 8999

CMD ["/backend/start.sh"]
