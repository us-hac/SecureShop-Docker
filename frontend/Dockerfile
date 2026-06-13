# Stage 1 — Build the React app
FROM node:20-alpine AS build

WORKDIR /app

# Copy package files first for dependency caching
COPY package*.json ./
RUN npm install

# Copy source code and build
COPY . .
RUN npm run build

# Stage 2 — Serve with nginx
FROM nginx:alpine

# Copy built files to nginx html folder
COPY --from=build /app/dist /usr/share/nginx/html

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]