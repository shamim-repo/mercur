FROM node:20-alpine

# Required for some native deps
RUN apk add --no-cache libc6-compat openssl

WORKDIR /app

# Copy dependency files first (better caching)
COPY package.json yarn.lock ./

# Install dependencies
RUN yarn install --frozen-lockfile

# Copy app source
COPY . .

# Build Medusa
RUN yarn build

# Medusa default port
EXPOSE 9000

# Start Medusa
CMD ["yarn", "start"]
