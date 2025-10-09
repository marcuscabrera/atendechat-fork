# syntax=docker/dockerfile:1.4

FROM node:18-slim AS builder

ENV NPM_CONFIG_FUND=false \
    NPM_CONFIG_AUDIT=false

WORKDIR /app

COPY package*.json ./
RUN npm install

COPY public ./public
COPY src ./src
COPY .env.exemple ./.env.exemple

ARG REACT_APP_BACKEND_URL=http://localhost:8080
ARG REACT_APP_HOURS_CLOSE_TICKETS_AUTO=24
ENV REACT_APP_BACKEND_URL=$REACT_APP_BACKEND_URL \
    REACT_APP_HOURS_CLOSE_TICKETS_AUTO=$REACT_APP_HOURS_CLOSE_TICKETS_AUTO

RUN printf "REACT_APP_BACKEND_URL=%s\nREACT_APP_HOURS_CLOSE_TICKETS_AUTO=%s\n" "$REACT_APP_BACKEND_URL" "$REACT_APP_HOURS_CLOSE_TICKETS_AUTO" > .env.production

RUN npm run build

FROM nginx:1.25-alpine AS runner

COPY --from=builder /app/build /usr/share/nginx/html

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
