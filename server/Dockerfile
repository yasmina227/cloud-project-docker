FROM node:20-alpine

WORKDIR /workspace

COPY package.json package-lock.json ./
COPY server/package.json ./server/package.json
COPY shared/package.json ./shared/package.json
COPY shared ./shared

RUN npm ci --workspace=server --workspace=shared --include-workspace-root=false

COPY server ./server

RUN npm run build --workspace=server

WORKDIR /workspace/server

EXPOSE 5000

CMD ["npm", "start"]