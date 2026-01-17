# -------- Base --------
FROM node:20-alpine AS base
WORKDIR /app

# Enable Corepack (for Yarn)
RUN corepack enable

# -------- Dependencies --------
FROM base AS deps
COPY package.json yarn.lock ./
RUN yarn install --immutable

# -------- Build --------
FROM base AS builder
COPY --from=deps /app/node_modules ./node_modules
COPY . .
RUN yarn build

# -------- Runtime --------
FROM node:20-alpine AS runner
WORKDIR /app

ENV NODE_ENV=production

# Enable Corepack in runtime too
RUN corepack enable

# Copy necessary files
COPY --from=builder /app/.next ./.next
COPY --from=builder /app/public ./public
COPY --from=builder /app/node_modules ./node_modules
COPY --from=builder /app/package.json ./package.json
COPY --from=builder /app/next.config.* ./

EXPOSE 3000

CMD ["yarn", "start"]
