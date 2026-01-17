# ========================
# Stage 1: Builder
# ========================
FROM node:20-alpine AS builder
WORKDIR /app

# Enable Corepack (required for Yarn v4)
RUN corepack enable

# Copy package files + Yarn config + releases (must exist)
COPY package.json yarn.lock .yarnrc.yml .yarn/releases/ ./

# Install dependencies (immutable ensures reproducible builds)
RUN yarn install --immutable

# Copy source code
COPY . .

# Build the Vite app
RUN yarn build

# ========================
# Stage 2: Production / Nginx
# ========================
FROM nginx:alpine AS runner
WORKDIR /usr/share/nginx/html

# Remove default nginx static files
RUN rm -rf ./*

# Copy the build output from builder
COPY --from=builder /app/dist ./

# SPA fallback for React Router / Vite
# This requires nginx.conf file
COPY nginx.conf /etc/nginx/conf.d/default.conf

# Expose port
EXPOSE 80

# Start nginx
CMD ["nginx", "-g", "daemon off;"]
