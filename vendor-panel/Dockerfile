FROM node:20-alpine

WORKDIR /app/vendor

# 1. Enable Corepack (Fixes the Yarn version mismatch)
RUN corepack enable

COPY package.json yarn.lock ./

RUN yarn install

COPY . .

RUN yarn build

EXPOSE 7002
CMD ["yarn", "start"]