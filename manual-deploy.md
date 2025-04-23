# Руководство по локальному развертыванию приложения

## Локальные зависимости

> **Примечание:** Данное руководство разработано для Ubuntu 24.04 ARM64. При использовании другой операционной системы или архитектуры процессора некоторые команды могут отличаться.

### Установка из репозиториев

```
Установка node.js
https://nodejs.org/en/download

Установка Golang
https://go.dev/doc/install

Установка redis-server
https://redis.io/docs/latest/operate/oss_and_stack/install/archive/install-redis/

Установка postgre
https://www.postgresql.org/download/
```

### Установка из CLI

```bash
# upd & upgd
sudo apt update && sudo apt upgrade -y

# установка golang
sudo apt install golang-go

# установка node.js и npm 
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt install -y nodejs

# установка postgresql
sudo apt install postgresql postgresql-contrib

# установка redis-server
sudo apt install redis-server
```

## Настройка компонентов

### Настройка PostgreSQL

```bash
# Запуск PostgreSQL
sudo systemctl start postgresql
sudo systemctl enable postgresql

# Создание пользователя и базы данных
sudo -u postgres psql -c "CREATE USER myuser WITH PASSWORD 'mypassword';"
sudo -u postgres psql -c "CREATE DATABASE mydb;"
sudo -u postgres psql -c "GRANT ALL PRIVILEGES ON DATABASE mydb TO myuser;"
```

### Настройка Redis

```bash
# Запуск Redis
sudo systemctl start redis-server
sudo systemctl enable redis-server

# Проверка работы Redis
redis-cli ping
# Должен получиться ответ PONG
```

## Запуск приложения

### Запуск бэкенда на Golang

```bash
# Переходим в директорию с бэкендом
cd /path/to/file

# Установка зависимостей Go
go mod download

# Сборка
go build -o app

# Запуск
./app
```

### Запуск фронтенда на Next.js

```bash
# Переходим в директорию с фронтендом
cd /path/to/file

# Установка зависимостей npm
npm install

# Запуск в режиме разработки
npm run dev

# ИЛИ для продакшн-сборки
npm run build
npm start
```

### Запуск всех компонентов вместе (скриптом)

```bash
#!/bin/bash

# Запуск PostgreSQL и Redis
sudo systemctl start postgresql
sudo systemctl start redis-server

# Запуск бэкенда
cd /path/to/file
./app &

# Запуск фронтенда
cd /path/to/file
npm start &

echo "Все сервисы запущены"
```
