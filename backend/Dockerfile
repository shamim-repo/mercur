<<<<<<< HEAD
FROM node:20-alpine

# Required for some native deps
RUN apk add --no-cache libc6-compat openssl

WORKDIR /app

# Copy dependency files first (better caching)
=======
# STAGE 1: Builder
FROM node:20-alpine AS builder
WORKDIR /app/backend
RUN corepack enable

>>>>>>> f83231d93bd97e39186d60d20159c4c7ac49890b
COPY package.json yarn.lock ./

# Install dependencies
RUN yarn install --frozen-lockfile

# Copy app source
COPY . .

# Build Medusa
RUN yarn build

<<<<<<< HEAD
# Medusa default port
EXPOSE 9000

# Start Medusa
CMD ["yarn", "start"]
=======
# STAGE 2: Runner
FROM node:20-alpine AS runner
WORKDIR /app/backend
ENV NODE_ENV=production
RUN corepack enable

# Install global Medusa CLI
RUN npm install -g @medusajs/medusa-cli

# Copy built backend and production dependencies
COPY --from=builder /app/backend/dist ./dist
COPY --from=builder /app/backend/node_modules ./node_modules
COPY --from=builder /app/backend/package.json ./package.json
# Copy other necessary config files if they exist (medusa-config.js, etc.)
COPY --from=builder /app/backend/medusa-config.js ./medusa-config.js

EXPOSE 9000
CMD ["sh", "-c", "medusa migrations run && medusa start"]
>>>>>>> f83231d93bd97e39186d60d20159c4c7ac49890b
