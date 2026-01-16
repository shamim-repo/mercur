FROM node:20-alpine

WORKDIR /app/vendor
COPY package.json yarn.lock ./
RUN yarn install --frozen-lockfile

COPY . .

# Build Next.js/Vite app
RUN yarn build

EXPOSE 7002
CMD ["yarn", "start"]