FROM node:23-alpine3.20 AS builder

WORKDIR /app

COPY package.json ./

RUN npm install

COPY . .
RUN sed -i "/const nextConfig/a \  output: 'standalone'," next.config.ts

RUN npm run build


FROM node:23-alpine3.20

WORKDIR /app

RUN apk add --no-cache curl

COPY --from=builder /app/public /app/public
COPY --from=builder /app/.next/standalone ./
COPY --from=builder /app/.next/static ./.next/static

HEALTHCHECK --interval=20s --timeout=3s --start-period=5s --retries=5 \
  CMD curl -f http://localhost:3000/healthz || exit 1

ENV HOSTNAME="0.0.0.0"

CMD ["node", "server.js"]
