# Use a lightweight Node.js base image
FROM node:18-alpine

# Set the working directory
WORKDIR /app

# Copy only package files first to leverage Docker caching
COPY package*.json ./

# Install only production dependencies
RUN npm ci --only=production

# Copy the rest of the application code
COPY [".", "./"]
COPY ["./.env.production", "./.env"]

# Expose the port the app runs on
EXPOSE 3000

# Start the application
CMD ["npm", "run", "start", "--", "--host", "0.0.0.0"]
