# Build Stage
FROM node:16.17.0-alpine AS builder

WORKDIR /app

# Copy dependency files first
COPY package.json package-lock.json* ./

# Install dependencies
RUN npm install

# Copy source code
COPY . .

# Build arguments
ARG TMDB_V3_API_KEY
ENV VITE_APP_TMDB_V3_API_KEY=${TMDB_V3_API_KEY}
ENV VITE_APP_API_ENDPOINT_URL="https://api.themoviedb.org/3"

# Build application
RUN npm run build


# Production Stage
FROM nginx:stable-alpine

WORKDIR /usr/share/nginx/html

# Clean default files
RUN rm -rf ./*

# Copy build output
COPY --from=builder /app/dist .

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
