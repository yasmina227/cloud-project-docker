# Stage 1: Build the React application
FROM node:20-alpine AS builder

WORKDIR /app

# Copy workspace configuration
COPY package.json package-lock.json ./
COPY client/package.json ./client/package.json
COPY shared/package.json ./shared/package.json
COPY server/package.json ./server/package.json

# Copy shared source files before installing dependencies
COPY shared ./shared

# Install client and shared dependencies
RUN npm ci --workspace=client --workspace=shared --include-workspace-root=false --ignore-scripts

# Copy client source code
COPY client ./client

# Build shared package first, then client
RUN npm run build --workspace=shared && npm run build --workspace=client


# Stage 2: Production
FROM nginx:alpine

COPY --from=builder /app/client/dist /usr/share/nginx/html

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]