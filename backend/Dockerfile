# STAGE 1: Builder
FROM node:20-alpine AS builder
WORKDIR /app/backend
RUN corepack enable

COPY package.json yarn.lock ./
RUN yarn install --immutable

COPY . .
RUN yarn build

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
