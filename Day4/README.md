# Day 4

## Info - Openshift S2I
<pre>
- Openshift introducted a new feature called S2I ( Source to Image )
- Kubernetes only supports deploying applications from readily available container images
- Openshift allows deployment application from
  - container image
  - source code
  - Dockerfile
</pre>

## Lab - Deploying application into openshift using docker strategy
```
oc project jegan
oc new-app https://github.com/tektutor/hello-microservice.git --strategy=docker
oc logs -f bc/hello-microservice
oc expose service/hello-microservice
```
Expected output
![image](https://github.com/user-attachments/assets/a2c744cf-b9e1-46bd-9553-28ea89e22864)
![image](https://github.com/user-attachments/assets/d31a061b-2e20-4ac6-a73c-68a498082415)
![image](https://github.com/user-attachments/assets/23d176b1-5377-47c5-9a4d-a4b5bfc5a1b4)

Navigate to Openshift Dashboard, select your project to see the output
![image](https://github.com/user-attachments/assets/f74ecd70-3c95-4415-87d2-276eea608ae1)
![image](https://github.com/user-attachments/assets/28f52032-c306-4cad-953c-08dc44ceeebb)


####
<pre>
- Whenever we deploy application from GitHub source code, openshift creates a BuildConfig
- The Build config captures GitHub repository url, strategy that must used to build the application and container image
- Once the image build is successfully completed, it will push the custom container image that has your application binary into Openshift's Internal Registry
- Now from the Openshift's Internal Registry, it will try to deploy your application
</pre>


## Lab - Deleting an Openshift project
```
oc delete project jegan
```

Expected output
![image](https://github.com/user-attachments/assets/a5f49ee6-b408-4714-b4b0-a41ade8ff81c)

## Lab - Understanding the purpose of Custom Container Image and Dockerfile 
Let's create a container
```
docker run -dit --name ubuntu1 --hostname ubuntu1 ubuntu:latest /bin/bash
docker ps
```

Getting inside the container
```
docker exec -it ubuntu1 /bin/bash
javac -version
java -version
mvn --version
tree
curl
git -version
```

Now we understand that the ubuntu:latest image we downloaded from hub.docker.com doesn't have the tools we need, so let's create a custom image using ubuntu:latest as the base image.

Let's create a Dockerfile
```
cd ~
mkdir CustomDockerImage
touch Dockerfile
```

Update the ~/CustomDockerImage/Dockerfile with the below content
<pre>
FROM ubuntu:latest

RUN apt update && apt install -y default-jdk maven git tree curl net-tools iputils-ping
</pre>

Let's build a custom docker image using the Dockerfile as shown below
```
cd ~/CustomDockerImage
docker build -t tektutor/maven:1.0 .
```

Expected output
![image](https://github.com/user-attachments/assets/50c9ed29-aeb8-489b-acae-ac749c8a25ec)

