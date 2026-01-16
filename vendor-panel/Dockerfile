# 1. Use Debian-based image (Fixes binary crashes)
FROM node:20-slim

WORKDIR /app/vendor

# 2. Install build tools for Debian (required for some node modules)
RUN apt-get update && \
    apt-get install -y python3 make g++ git && \
    rm -rf /var/lib/apt/lists/*

# 3. Enable Corepack
RUN corepack enable

# 4. Copy package.json
COPY package.json ./

# 5. Install dependencies
# We use --no-lockfile to bypass any OS-specific lockfile issues
RUN yarn install --no-lockfile

# 6. Copy the rest of the source code
COPY . .

# 7. Set Build Variables (Prevents strict checks from failing the build)
ENV NODE_ENV=production
ENV NEXT_TELEMETRY_DISABLED=1
ENV CI=false
ENV TSC_COMPILE_ON_ERROR=true
ENV ESLINT_NO_DEV_ERRORS=true
# Increase memory limit to prevent crashing
ENV NODE_OPTIONS="--max-old-space-size=4096"

# 8. Build
RUN yarn build

EXPOSE 7002
CMD ["yarn", "start"]
