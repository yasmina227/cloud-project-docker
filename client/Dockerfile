FROM node:20-alpine AS builder

WORKDIR /workspace

COPY package.json package-lock.json ./
COPY client/package.json ./client/package.json
COPY server/package.json ./server/package.json
COPY shared/package.json ./shared/package.json

COPY shared ./shared

RUN npm ci --workspace=client --workspace=shared --include-workspace-root=false --ignore-scripts

COPY client ./client

RUN npm run build --workspace=shared && npm run build --workspace=client

FROM nginx:alpine

COPY client/nginx.conf /etc/nginx/conf.d/default.conf
COPY --from=builder /workspace/client/dist /usr/share/nginx/html

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]