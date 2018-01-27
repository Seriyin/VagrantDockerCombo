# Use an official Ubuntu image as a parent image
FROM ubuntu:16.04

WORKDIR ~/


# Install any needed packages
RUN apt-get update && apt-get -y upgrade 
RUN apt-get -y install software-properties-common curl 
RUN add-apt-repository ppa:webupd8team/java
RUN apt-get -y update 
RUN echo "oracle-java7-installer shared/accepted-oracle-license-v1-1 boolean true" | debconf-set-selections 
RUN apt-get -y install oracle-java8-installer 
RUN curl -O http://archive.apache.org/dist/tomcat/tomcat-7/v7.0.55/bin/apache-tomcat-7.0.55.tar.gz
RUN tar xzf apache-tomcat-7.0.55.tar.gz

COPY ./myapps /apache-tomcat-7.0.55/webapps/

# Make port 8080 available to the world outside this container
EXPOSE 8080

# Run app.py when the container launches
CMD apache-tomcat-7.0.55/bin/startup.sh && tail -f apache-tomcat-7.0.55/logs/catalina.out