FROM golang:1.22-alpine as builder

ARG TARGETOS
ARG TARGETPLATFORM
ARG TARGETARCH
ARG CADDY_VERSION="v2.7.6"
ENV CADDY_VERSION=$CADDY_VERSION
RUN echo building caddy $CADDY_VERSION for "$TARGETOS"

WORKDIR /workspace

RUN go install github.com/caddyserver/xcaddy/cmd/xcaddy@latest

RUN GOOS=$TARGETOS GOARCH=$TARGETARCH xcaddy build \
    --with github.com/abiosoft/caddy-yaml \
    --with github.com/mholt/caddy-l4 \
    --with github.com/pberkel/caddy-storage-redis

FROM alpine

COPY --from=builder /workspace/caddy /bin/caddy

# Run the binary.
ENTRYPOINT ["/bin/caddy"]
