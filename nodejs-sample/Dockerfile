# Stage 1: Build
FROM node:16 AS builder
WORKDIR /app
COPY package.json ./
RUN npm install
COPY . .
RUN echo "Building the app..." 

# Stage 2: Production Image
FROM node:16-alpine
WORKDIR /app
COPY --from=builder /app .
CMD ["node", "index.js"]
