<<<<<<< HEAD
# -------- Stage 1: Build --------
FROM node:20-alpine AS builder
WORKDIR /app

# Enable Corepack
RUN corepack enable

# Copy package files and install deps
COPY package.json yarn.lock ./
RUN yarn install --immutable

# Copy source code
COPY . .

# Build the Vite app
RUN yarn build

# -------- Stage 2: Serve --------
FROM nginx:alpine AS runner
WORKDIR /usr/share/nginx/html

# Remove default nginx static files
RUN rm -rf ./*

# Copy build output
COPY --from=builder /app/dist ./

# Optional: SPA fallback for React Router / Vite
COPY nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
=======
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
>>>>>>> f83231d93bd97e39186d60d20159c4c7ac49890b
