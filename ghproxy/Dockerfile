FROM golang:1.24-alpine AS builder

WORKDIR /app

COPY . .

ARG VERSION=2.5.0

RUN CGO_ENABLED=0 go build -ldflags "-X main.version=${VERSION} -X main.dev=true" -o main ./main.go

FROM alpine:latest

RUN apk --no-cache add ca-certificates curl

WORKDIR /data

COPY --from=builder /app/main /data/

RUN mkdir -p ghproxy/config ghproxy/log www \
    && touch ghproxy/log/run.log

COPY --from=builder /app/config /data/ghproxy/config/


RUN chmod +x /data/main

CMD ["/data/main", "-cfg", "/data/ghproxy/config/config.toml"]