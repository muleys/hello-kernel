FROM java:openjdk-8-jdk

MAINTAINER amishra@atlassian.com

WORKDIR /opt

#RUN apk update && apk add ca-certificates wget && update-ca-certificates
RUN apt-get update -y && apt-get install maven -y 
#apt-get install git

#RUN cd /opt && tar xvzf ~/mule-standalone-4.1.1.tar.gz && rm ~/mule-standalone-4.1.1.tar.gz && ln -s /opt/mule-standalone-4.1.1 /opt/mule
RUN cd ~ && wget https://repository-master.mulesoft.org/nexus/content/repositories/releases/org/mule/distributions/mule-standalone/4.1.1/mule-standalone-4.1.1.tar.gz
RUN cd /opt && tar xvzf ~/mule-standalone-4.1.1.tar.gz && rm ~/mule-standalone-4.1.1.tar.gz

#COPY /Users/amishra/Downloads/mule-standalone-4.1.1/apps/hello-mule4-kernel.jar /opt/mule/apps
RUN git clone ssh://git@stash.atlassian.com:7997/MOE/mule4-kernel-hello-app.git
RUN ["pwd"]


RUN ["ls"]

RUN cd mule4-kernel-hello-app
RUN ["pwd"]
RUN  mvn clean install

# Define environment variables.
ENV MULE_HOME /opt/mule-standalone-4.1.1

# Define mount points.
VOLUME ["/opt/mule-standalone-4.1.1/logs", "/opt/mule-standalone-4.1.1/conf", "/opt/mule-standalone-4.1.1/apps", "/opt/mule-standalone-4.1.1/domains"]

# Define working directory.
WORKDIR /opt/mule-standalone-4.1.1

CMD [ "/opt/mule-standalone-4.1.1/bin/mule" ]

# Default http port
EXPOSE 8080