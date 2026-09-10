# ---- Build stage: has npm, used only to install deps ----
FROM node:22-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci --omit=dev
COPY . .

# ---- Runtime stage: patched OS, no npm CLI at all ----
FROM node:22-alpine
RUN apk update && apk upgrade --no-cache
WORKDIR /app
COPY --from=builder /app/node_modules ./node_modules
COPY --from=builder /app ./

# Strip npm/npx/corepack/yarn and their bundled node_modules —
# not needed at runtime, and this is where the CVEs were hiding
RUN rm -rf /usr/local/lib/node_modules/npm \
    /usr/local/lib/node_modules/corepack \
    /usr/local/bin/npm /usr/local/bin/npx /usr/local/bin/corepack \
    /opt/yarn-v*

USER node
EXPOSE 3000
CMD ["node", "src/server.js"]
