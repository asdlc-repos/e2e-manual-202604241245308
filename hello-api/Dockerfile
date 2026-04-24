FROM golang:1.21-alpine AS builder

WORKDIR /src

COPY go.mod ./
RUN go mod download

COPY . .

RUN CGO_ENABLED=0 GOOS=linux go build -ldflags="-s -w" -o /app/hello-api .

FROM alpine:latest

RUN apk --no-cache add ca-certificates && \
    addgroup -S app && adduser -S app -G app

WORKDIR /app

COPY --from=builder /app/hello-api /app/hello-api

USER app

EXPOSE 9090

ENTRYPOINT ["/app/hello-api"]
