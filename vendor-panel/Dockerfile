# Use Debian-slim (More stable than Alpine for building native modules)
FROM node:20-slim

# 1. Install Build Tools & Git
RUN apt-get update && \
    apt-get install -y python3 make g++ git && \
    rm -rf /var/lib/apt/lists/*

WORKDIR /app

# 2. CRITICAL FIX: Force Yarn to use 'node_modules' folder
# This disables PnP, which is causing the esbuild failure.
ENV YARN_NODE_LINKER=node-modules

# 3. Copy the ENTIRE repository
# We need .yarn, .yarnrc.yml, patches, and the root package.json
COPY . .

# 4. Enable Corepack and Install Dependencies from ROOT
RUN corepack enable
# We use --immutable to ensure we respect the lockfile, 
# but if that fails, remove --immutable to force a fix.
RUN yarn install

# 5. Move to the specific service folder to build
WORKDIR /app/vendor


# 7. Build the specific service
RUN yarn build

EXPOSE 7002
CMD ["yarn", "start"]
