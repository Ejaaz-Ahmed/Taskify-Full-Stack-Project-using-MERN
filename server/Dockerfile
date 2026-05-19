FROM node:18-alpine

WORKDIR /app

# Install only production dependencies
COPY package*.json ./
RUN npm ci --omit=dev

# Copy application source
COPY . .

# Match local API port used by Vite proxy/docker runs
EXPOSE 8800

# Server entrypoint is at repository root
CMD ["node", "index.js"]