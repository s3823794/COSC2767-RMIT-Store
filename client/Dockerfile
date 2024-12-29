# Stage 1: Build the application
FROM node:18-alpine AS build

ARG API_URL
ARG REACT_APP_API_URL

# Set the working directory
WORKDIR /app

RUN rm -rf /app/*
COPY . ./
COPY ["./.env.production", "./.env"]
COPY package*.json ./

# Install dependencies
RUN npm ci --legacy-peer-deps

# Build the production-ready static files
RUN npm run build

# Stage 2: Serve the static files with Nginx
FROM nginx:alpine AS production

# Copy build output from the previous stage
RUN rm -rf /usr/share/nginx/html/*
COPY --from=build /app/build /usr/share/nginx/html

# Expose the port Nginx serves on
EXPOSE 80

# Start Nginx
CMD ["nginx", "-g", "daemon off;"]
