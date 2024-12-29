# Stage 1: Build the application
FROM node:18-alpine AS build

# Set the working directory
WORKDIR /app

# Copy only package files first for better caching
COPY package*.json ./

# Install dependencies
RUN npm ci --legacy-peer-deps

# Copy the rest of the application code
COPY . .

# Build the production-ready static files
RUN npm run build

# Stage 2: Serve the static files with Nginx
FROM nginx:alpine AS production

# Copy build output from the previous stage
COPY --from=build /app/build /usr/share/nginx/html

# Expose the port Nginx serves on
EXPOSE 8080

# Start Nginx
CMD ["nginx", "-g", "daemon off;"]
