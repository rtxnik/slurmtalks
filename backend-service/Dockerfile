# Build app
FROM golang:1.24.0-alpine AS builder

WORKDIR /app

COPY go.mod go.sum .
RUN go mod download

COPY . . 
RUN CGO_ENABLED=0 GOOS=linux go build -o backend-service .

# Run app
FROM alpine:latest AS runner

RUN apk --no-cache add ca-certificates tzdata

WORKDIR /app

COPY --from=builder /app/ .

EXPOSE 8080

CMD ["./app"]
