# syntax=docker/dockerfile:1.4

FROM node:18-slim AS builder

ENV NPM_CONFIG_FUND=false \
    NPM_CONFIG_AUDIT=false

WORKDIR /app

COPY package*.json ./
COPY tsconfig.json ./
RUN npm install

COPY src ./src
COPY certs ./certs

RUN npm run build
RUN npm prune --production

FROM node:18-slim AS runner

ENV NODE_ENV=production \
    PUPPETEER_EXECUTABLE_PATH=/usr/bin/chromium
WORKDIR /app

# Install system dependencies required at runtime
RUN apt-get update \
  && apt-get install -y --no-install-recommends chromium ffmpeg tini \
  && rm -rf /var/lib/apt/lists/*

COPY --from=builder /app/package*.json ./
COPY --from=builder /app/node_modules ./node_modules
COPY --from=builder /app/dist ./dist
COPY --from=builder /app/certs ./certs
COPY .sequelizerc ./.sequelizerc

EXPOSE 8080

ENTRYPOINT ["tini", "--"]
CMD ["sh", "-c", "npm run db:migrate && node dist/server.js"]
