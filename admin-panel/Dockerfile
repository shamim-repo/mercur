FROM node:20-alpine AS builder

WORKDIR /app/admin
COPY package.json yarn.lock ./
RUN yarn install --frozen-lockfile
COPY . .
# Build the admin panel (Vite/React)
RUN yarn build

FROM node:20-alpine AS runner
WORKDIR /app/admin
RUN npm install -g serve
COPY --from=builder /app/admin/dist ./dist

EXPOSE 7001
CMD ["serve", "-s", "dist", "-l", "7001"]