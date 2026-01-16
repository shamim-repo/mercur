FROM node:20-alpine
WORKDIR /app/backend

# Fix: Enable Yarn 3
RUN corepack enable

# Install global CLI (optional, but good for Medusa)
RUN npm install -g @medusajs/medusa-cli

COPY package.json yarn.lock ./
RUN yarn install --immutable

COPY . .
RUN yarn build

EXPOSE 9000
CMD ["sh", "-c", "medusa migrations run && medusa start"]