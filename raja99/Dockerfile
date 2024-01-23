# Use an official Nginx image as a base image
FROM nginx:latest

# Remove the default index.html
RUN rm ./usr/share/nginx/html/*

#Working Dir
WORKDIR /app

# Copy your custom index.html to the Nginx public directory
COPY ./inance-html/ /usr/share/nginx/html/

# Expose port 80
EXPOSE 80
