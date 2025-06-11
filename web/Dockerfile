FROM node:20-alpine AS deps
WORKDIR /app
COPY web/package*.json ./
RUN npm ci --production

FROM node:20-alpine AS builder
WORKDIR /app
COPY --from=deps /app/node_modules ./node_modules
COPY web .
RUN npm run build

FROM node:20-alpine
WORKDIR /app
COPY --from=builder /app/.next ./.next
COPY --from=builder /app/public ./public
COPY --from=deps  /app/node_modules ./node_modules
ENV NODE_ENV=production
EXPOSE 3000
CMD ["npx","next","start"]
