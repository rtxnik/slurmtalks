# Этап сборки приложения
FROM golang:1.24.0-alpine AS builder
WORKDIR /app
# Копируем файлы зависимостей
COPY go.mod go.sum ./
RUN go mod download
# Копируем исходный код и собираем приложение
COPY . . 
RUN CGO_ENABLED=0 GOOS=linux go build -o backend-service .

# Этап запуска приложения
FROM alpine:latest AS runner
# Добавляем необходимые пакеты
RUN apk --no-cache add ca-certificates tzdata
WORKDIR /app
# Копируем результаты сборки из первого этапа
COPY --from=builder /app/ ./
EXPOSE 8080
CMD ["./app"]
