FROM node:20-alpine

WORKDIR /app/backend

# Install global dependencies
RUN npm install -g @medusajs/medusa-cli

# Copy package files
COPY package.json yarn.lock ./

# Install dependencies
RUN yarn install --frozen-lockfile

# Copy source code
COPY . .

# Build the backend
RUN yarn build

# Expose port (default Medusa port)
EXPOSE 9000

# Start command (Runs migrations then starts server)
CMD ["sh", "-c", "medusa migrations run && medusa start"]