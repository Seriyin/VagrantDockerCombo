# Use an official Ubuntu image as a parent image
FROM nginx:latest

WORKDIR ~/


# Install any needed packages
RUN apt-get update && apt-get install -qy ssl-cert 

COPY ./virtualhost.conf /etc/nginx/conf.d/default.conf

# Make ports 80 and 443 available to the world outside this container
EXPOSE 80
EXPOSE 443
