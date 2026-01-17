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
