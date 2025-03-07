# Day 5

## Info - Openshift Internal Image Registry
<pre>
- When we install Openshift, we also deploy Openshift Internal Registry to store multiple container images in it
- Whenever we deploy applications using 'oc create' or 'oc new-app' or using Openshift webconsole, the images we mention, openshift will search the image first in the Openshift Internal Registry, if the image is not there, then it downloads from the Docker Hub or whatever url is mentioned in the container imaage.
- We can thing of Openshift Internal Image Registry as server that stores many container images
</pre>  

## Info - Openshift ImageStream
<pre>
- Image Stream is a folder we create within the Openshift Internal Image Registry to store a particular image
- Image Stream will allow us to store multiple versions of a single container image
- For example
  - Assume we have created a ImageStream with a name hello
  - We can store image version hello:1.0, hello:2.0, hello:3.0 within the image stream named hello
</pre>

## Demo - Importing docker image from Docker Hub Repository to Openshift's Internal registry ( Kindly avoid importing additional images to avoid exhausting the docker limit )
```
docker login

oc create secret generic docker-pullsecret --from-file=.dockerjsonfile=/root/.docker/config.json --type=kubernetes.io/dockerconfigjson

oc import-image bitnami/nginx:latest bitnami/nginx:latest docker.io/library/bitnami/nginx:latest --confirm

oc get images
```

Expected output
![output](img1.png)
![output](img5.png)
![output](img2.png)
![output](img3.png)
![output](img4.png)
![output](img6.png)
![output](img7.png)

For more details, you may also refer official documentation here
<pre>
https://access.redhat.com/solutions/2072843  
</pre>

## Lab - Deploying application into Openshift using docker strategy
<pre>
- In case of docker strategy, openshift will expect for Dockerfile in your GitHub repository.  
- Openshift will use the Dockerfile to perform the application and image build.
</pre>

```
oc project jegan
oc new-app https://github.com/tektutor/hello-microservice.git --strategy=docker
```

## Lab - Deploying application into Openshift using source strategy
<pre>
- In case of source strategy, openshift will not expect you to provide Dockerfile in your GitHub repository. 
- Even if there is Dockerfile in the repository, Openshift will generate a Dockerfile with the image name you suggested in the oc new-app command.  
- Openshift will use the Dockerfile to perform the application and image build.
</pre>


```
oc project jegan
oc new-app --name=hello https://github.com/tektutor/hello-microservice.git --strategy=source
```

Expected output
![image](https://github.com/user-attachments/assets/805ff747-7441-472a-967d-3758478ce8b5)
![image](https://github.com/user-attachments/assets/dd6db220-76a1-4dff-9afd-603028edc62c)
![image](https://github.com/user-attachments/assets/a333bc45-2b8c-4be6-883e-e28defbf5b4d)
![image](https://github.com/user-attachments/assets/dcd86cb2-59e2-40f5-afc9-8697d311c372)
![image](https://github.com/user-attachments/assets/fe8036e0-945f-4652-987c-691d1cd40c0d)

## Lab - Deploying an application into openshift using existing images that we stored in Openshift Internal Image Registry
Navigate to Openshift Developer context as shown below, Click +Add on the left side menu
![image](https://github.com/user-attachments/assets/4c956874-c246-4eda-8454-0950fec694e7)
Click on "Container images"
![image](https://github.com/user-attachments/assets/1ee57a65-cf22-4728-96a0-08983a6c08d4)
Select "Image stream tag from Internal registry"
![image](https://github.com/user-attachments/assets/3430a73f-f001-4d60-9860-89fea7b65e5f)
Select the Project from which you wish to choose the Imagestream, 
Select the Image stream name and tag
![image](https://github.com/user-attachments/assets/fcebc5fd-30bb-40a1-9a02-e999832f4879)
Scroll down, make the "Create a route" checkbox is selected
![image](https://github.com/user-attachments/assets/df961003-ee77-401a-99ca-28cd587c3f62)
Click "Create" button
![image](https://github.com/user-attachments/assets/2429d77f-566f-4829-8276-cf1a1e9e7fbe)
![image](https://github.com/user-attachments/assets/a47a268d-6e6f-4a72-b2bf-4ee8d311074e)
Click the "Up arrow"
![image](https://github.com/user-attachments/assets/716453cf-999a-46ad-90f1-9a79c42795c1)
![image](https://github.com/user-attachments/assets/e0e5584b-059e-433d-bb84-d331a0a9ae10)

## Lab - Creating a NodePort external service
```
oc project jegan
oc new-app --name=hello --image=image-registry.openshift-image-registry.svc:5000/jegan/hello-microservice
oc get po
oc scale deploy/hello --replicas=3
oc get po

```
Expected output
![image](https://github.com/user-attachments/assets/22b8a355-691a-47f1-8bfb-34eb612662d9)
![image](https://github.com/user-attachments/assets/4c1ccfc2-70e7-4928-b787-b0c18bd33565)
![image](https://github.com/user-attachments/assets/4e0a8697-1395-46b4-9a0c-96e3b1e4559c)

Let's create the external node-port service
```
oc get svc
oc delete svc/hello
oc expose deploy/hello --port=8080 --type=NodePort
oc get services
oc describe service/hello
oc get nodes -o wide
curl http://192.168.100.11:30397
curl http://192.168.100.12:30397
curl http://192.168.100.13:30397
curl http://192.168.100.22:30397
curl http://192.168.100.23:30397
```

Expected output
![image](https://github.com/user-attachments/assets/b2aa467d-76f5-4ba8-a52b-d3642cc0afdf)
![image](https://github.com/user-attachments/assets/f5d8a961-834a-4b32-83ed-e736e85e0cc5)
![image](https://github.com/user-attachments/assets/a7917077-35f8-449f-82b3-67aaace7acb5)
![image](https://github.com/user-attachments/assets/21d3c123-7ea3-4f6f-b57f-5011110f628b)
![image](https://github.com/user-attachments/assets/7c5f8b41-41e7-4474-baae-a32f7703def3)
![image](https://github.com/user-attachments/assets/030412a1-a3eb-42f6-a143-9dc34d525bae)

## Demo - Installing Metallb operator to support LoadBalancer service in on-prem(bare-metal) openshift setup
To find the openshift webconsole url and login credentials
```
cat ~/openshift.txt
```
You could also find the webconsole url as shown below
```
oc whoami --show-console
```

Navigate to Openshift Dashboard as an Administrator
![image](https://github.com/user-attachments/assets/94005f06-8a10-4abf-bdb1-5f4c8878ce42)
Click "Operators" on the left side menu
![image](https://github.com/user-attachments/assets/905882b8-fd72-4c48-bbb0-78343c82d921)
Select "Operator Hub"
![image](https://github.com/user-attachments/assets/1bcfebb0-32aa-4ffd-86eb-433db0f186a6)
Search "Metal"
![image](https://github.com/user-attachments/assets/16af6666-a6f5-4d2d-83e2-5f7ed08679a0)
Select "MetalLB Operator"
![image](https://github.com/user-attachments/assets/845b3135-f902-4ea8-bc70-c860071c7854)
Click "Install"
![image](https://github.com/user-attachments/assets/04f4dd50-26f8-41c5-b1b0-319c5170fd42)
Let's go with the default options, hence no need to change anything
![image](https://github.com/user-attachments/assets/604eec9a-0184-4e6e-b84d-b649faee8480)
Scroll down and click "Install"
![image](https://github.com/user-attachments/assets/113d8739-624c-49a6-b9c3-f3f3b9c13cf6)
![image](https://github.com/user-attachments/assets/fabf5cc5-9acd-4e9b-89fd-0d3eb2c5bc97)
We need to configure Metallb Operator, Let's click "View operator"
![image](https://github.com/user-attachments/assets/b390997f-11c0-48c2-bf42-f1d60c0d93e6)
Click "IPAddressPool" Create Instance
![image](https://github.com/user-attachments/assets/d9e340b5-06aa-4d9a-87b6-749e205b2da1)
To find a range of IP address that is not in use, let's run the below command
```
oc get nodes -o wide
```
![image](https://github.com/user-attachments/assets/b14817cb-e83b-4de1-9a56-0134510dde4d)


