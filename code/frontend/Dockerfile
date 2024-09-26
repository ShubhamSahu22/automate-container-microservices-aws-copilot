# Stage 1: Build the React app with Node.js 18
FROM public.ecr.aws/bitnami/node:18 AS builder

# Create a directory for the web app
RUN mkdir -p /webapp
WORKDIR /webapp

# Install app dependencies
COPY webapp/package.json /webapp
RUN npm install

# Copy the app source code and build it
COPY webapp /webapp
RUN npm run build

# Stage 2: Set up Nginx to serve the static build files
FROM public.ecr.aws/nginx/nginx:latest

# Set working directory to the Nginx asset directory
WORKDIR /usr/share/nginx/html

# Remove default Nginx static assets
RUN rm -rf ./*

# Copy static assets from the builder stage
COPY backend.conf.template /etc/nginx/templates/backend.conf.template
COPY --from=builder /webapp/build .

# Set proper permissions for the copied assets
RUN chmod -R 777 .

# Expose port 80 for the application
EXPOSE 80

# Run Nginx in the foreground
CMD ["nginx", "-g", "daemon off;"]
