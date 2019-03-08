########################### Local System ########################### 
# Clear existing container and image in local 
docker container ps
docker container stop <>
docker container rm <>
docker images
docker image rm <>

#Delete existing micros service
micros service:delete hello-mule4-kernel

#Project root
cd /Users/amishra/AnypointStudio/studio-workspace/hello-mule4-kernel

#Build image from dockerfile on project root
docker build -t docker.atl-paas.net/${USER}/mule4-kernel:latest .

#Run container from the image
docker run -ti --rm -p 8080:8080 docker.atl-paas.net/${USER}/mule4-kernel:latest

#Get docker container Id
docker container ps

#copy mule app jar from local to container's mule apps folder
docker cp /Users/amishra/Downloads/mule-standalone-4.1.1/apps/hello-mule4-kernel.jar <docker_container_id>:/opt/mule-standalone-4.1.1/apps

#To run command inside a running container
docker exec -it <docker_container_id> /bin/bash

#Test Healthcheck
cd /opt/mule-standalone-4.1.1/logs
tail -f hello-mule4-kernel.log
curl https://localhost:8080/healthcheck

#Pusg docker image to docker registery
docker login docker.atl-paas.net
docker push docker.atl-paas.net/${USER}/mule4-kernel:latest

#Create micros service
micros user:login
micros service:create hello-mule4-kernel --no-sd

#deploy micros service 
DOCKER_IMAGE=docker.atl-paas.net/${USER}/mule4-kernel \
DOCKER_TAG=latest \
micros service:deploy hello-mule4-kernel -f service-descriptor.yml

#If deployment failed due to healthcheck
#$ micros service:deploy <service name> -e <env> -f service-descriptor.yml -u cfn-debug

DOCKER_IMAGE=docker.atl-paas.net/${USER}/mule4-kernel \
DOCKER_TAG=latest \
micros service:deploy hello-mule4-kernel -e ddev -f service-descriptor.yml -u cfn-debug

#get the host url for envs
micros service:show

########################### AWS SSH ########################### 

# SSH into AWS micros instance
ssh jumpbox.ap-southeast-2.dev.paas-inf.net.

#To get the IPs
ec2showservice hello-mule4-kernel

#SSH
ssh 10.116.60.83

#To get the container Id
docker container ps
docker exec -it <container_id> /bin/bash

#Download mule app jar
cd /opt
wget https://github.com/muleys/hello-kernel/blob/master/hello-mule4-kernel.jar

#Copy jar to mule apps folder
cp hello-mule4-kernel.jar 	

#Run Mule
export MULE_HOME=/opt/mule-standalone-4.1.1
cd /opt/mule-standalone-4.1.1/bin
./mule start


