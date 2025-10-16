# Use Nginx to serve static HTML
FROM nginx:alpine

# Copy HTML file to Nginx default folder
COPY Devops_Intern.html /usr/share/nginx/html/index.html

# Expose port 80
EXPOSE 80

# Start Nginx
CMD ["nginx", "-g", "daemon off;"]
