# Build
FROM rust:1.57.0-slim@sha256:43d8c9a7e8f0784372a4c0009b064b9f40051f4f950c7c96b9650ad8b445add4 as build

RUN apt-get update && apt-get install -y pkg-config libssl-dev cmake

WORKDIR /usr/src/radicle-client-services
COPY . .

WORKDIR /usr/src/radicle-client-services/http-api
RUN cargo install --all-features --locked --path .

# Run
FROM debian:bullseye-slim@sha256:96e65f809d753e35c54b7ba33e59d92e53acc8eb8b57bf1ccece73b99700b3b0

EXPOSE 8777/tcp
RUN apt-get update && apt-get install -y libssl1.1 && rm -rf /var/lib/apt/lists/*
COPY --from=build /usr/local/cargo/bin/radicle-http-api /usr/local/bin/radicle-http-api
WORKDIR /app/radicle
ENTRYPOINT ["/usr/local/bin/radicle-http-api", "--root", "/app/radicle/root", "--listen", "0.0.0.0:8777"]
