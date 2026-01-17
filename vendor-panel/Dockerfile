# -------- Stage 1: Build --------
FROM node:20-alpine AS builder
WORKDIR /app

# Enable Corepack
RUN corepack enable

# Copy package files and Yarn files
COPY package.json yarn.lock .yarnrc.yml .yarn/releases ./

# Install dependencies
RUN yarn install --immutable

# Copy the rest of the source code
COPY . .

# Build the Vite/React app
RUN yarn build

# -------- Stage 2: Serve --------
FROM nginx:alpine AS runner
WORKDIR /usr/share/nginx/html

RUN rm -rf ./*

COPY --from=builder /app/dist ./
COPY nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
