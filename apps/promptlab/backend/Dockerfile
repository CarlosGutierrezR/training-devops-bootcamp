FROM node:20-alpine AS builder

WORKDIR /app

RUN apk add --no-cache python3 make g++

COPY package*.json ./
RUN npm ci --omit=dev

FROM node:20-alpine

WORKDIR /app

RUN apk add --no-cache wget

COPY --from=builder /app/node_modules ./node_modules
COPY package*.json ./
COPY src/ ./src/

RUN mkdir -p /app/data

EXPOSE 3001

HEALTHCHECK --interval=30s --timeout=3s --start-period=5s \
  CMD wget --quiet --tries=1 --spider http://localhost:3001/health || exit 1

CMD ["node", "src/index.js"]
