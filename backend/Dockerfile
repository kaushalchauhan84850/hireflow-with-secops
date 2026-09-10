FROM node:22-alpine

WORKDIR /app

# Copy package files first for layer caching
COPY package*.json ./

# Clean production install strictly using package-lock.json
RUN npm ci --only=production

COPY . .

EXPOSE 3000

CMD ["npm", "start"]
