FROM node:24-alpine AS builder

WORKDIR /app

COPY package*.json ./
RUN npm ci --maxsockets=1

COPY tsconfig.json ./
COPY src ./src
RUN npx tsc

FROM node:24-alpine AS runner

WORKDIR /app

COPY package*.json ./
RUN npm ci --omit=dev --maxsockets=1

COPY --from=builder /app/dist ./dist

RUN chown -R node:node /app
USER node

EXPOSE 8080

CMD ["node", "dist/index.js"]
