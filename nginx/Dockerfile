FROM nginx:alpine

# Remove default nginx configuration
RUN rm /etc/nginx/nginx.conf

# Copy custom nginx configuration
COPY nginx.conf /etc/nginx/nginx.conf

# Create a simple index page for the root endpoint
RUN echo 'Nginx Reverse Proxy is running!\n\nAvailable endpoints:\n- /api/golang - Golang service\n- /api/python - Python service\n' > /usr/share/nginx/html/index.html

# Expose port 80
EXPOSE 80

# Start nginx
CMD ["nginx", "-g", "daemon off;"]