# syntax=docker/dockerfile:1
ARG DEBIAN_REPO
FROM golang:{{GO_VERSION}}-bookworm AS builder

ARG GOPROXY

# Build dependencies (add system deps if needed)
RUN set -eux; \
    apt-get update; \
    apt-get install -y --no-install-recommends \
      pkg-config; \
    rm -rf /var/lib/apt/lists/*

WORKDIR /app

COPY go.mod go.sum ./
RUN set -eux; \
    if [ -n "${GOPROXY:-}" ]; then go env -w GOPROXY="$GOPROXY"; fi; \
    go mod download

COPY . .

# Build all Go services under ./cmd/* that contain main.go
RUN --mount=type=cache,target=/root/.cache/go-build \
    --mount=type=cache,target=/go/pkg/mod \
    set -eux; \
    mkdir -p /out; \
    for d in ./cmd/*; do \
      [ -d "$d" ] && [ -f "$d/main.go" ] || continue; \
      name="$(basename "$d")"; \
      echo "building ${name}"; \
      CGO_ENABLED=1 GOOS=linux go build -ldflags="-w -s" -o "/out/${name}" "$d"; \
    done; \
    ls -lh /out

FROM debian:bookworm-slim

ARG DEBIAN_REPO
# Optional: switch Debian repo mirror at build time
RUN set -eux; \
    REPO="${DEBIAN_REPO:-}"; \
    if [ -n "$REPO" ]; then \
      if [ -f /etc/apt/sources.list ]; then \
        sed -E -i "s#(http(s)?://)[^/ ]+#\\1$REPO#g" /etc/apt/sources.list; \
      elif [ -f /etc/apt/sources.list.d/debian.sources ]; then \
        sed -E -i "s#(URIs:\\s*)(http(s)?://)[^/ ]+#\\1\\2$REPO#g" /etc/apt/sources.list.d/debian.sources; \
      fi; \
    fi; \
    apt-get update; \
    apt-get install -y --no-install-recommends \
      ca-certificates; \
    rm -rf /var/lib/apt/lists/*

WORKDIR /app

# Copy all service binaries into PATH
COPY --from=builder /out/ /usr/local/bin/

# Document ports (update to match your services)
EXPOSE 8080 9090

# Default entrypoint — override per service in docker-compose or K8s
ENTRYPOINT ["/bin/bash"]
CMD ["-c", "exec /usr/local/bin/{{SERVICE_NAME}}"]
