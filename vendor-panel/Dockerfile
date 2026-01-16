# STAGE 1: Builder
FROM node:20-alpine AS builder
WORKDIR /app/vendor
RUN corepack enable

# Copy deps and install
COPY package.json yarn.lock ./
RUN yarn install --immutable

# Copy source and build
COPY . .
# Pass dummy env vars to prevent build failure
ENV NODE_ENV=production
ENV VITE_MEDUSA_BACKEND_URL=http://backend:9000
RUN yarn build

# STAGE 2: Runner
FROM node:20-alpine AS runner
WORKDIR /app/vendor

# Install static file server
RUN npm install -g serve

# Copy built assets from builder
COPY --from=builder /app/vendor/dist ./dist

EXPOSE 7002
CMD ["serve", "-s", "dist", "-l", "7002"]
