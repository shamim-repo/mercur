FROM node:20-alpine
WORKDIR /app/storefront
RUN corepack enable  # <--- Add this

COPY package.json yarn.lock ./
RUN yarn install --immutable

COPY . .
RUN yarn build

EXPOSE 8000
CMD ["yarn", "start"]