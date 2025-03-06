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

## Lab - Deploying application into openshift using source strategy
```
oc project jegan
oc new-app https://github.com/tektutor/spring-ms.git --strategy=docker
oc logs -f bc/spring-ms
```
Expected output
![image](https://github.com/user-attachments/assets/8d66a789-9490-44f2-bc33-b91eac53af93)
![image](https://github.com/user-attachments/assets/5ec50c93-3b6d-41e6-8260-f4c0d688a427)


## Lab - Deleting an Openshift project
```
oc delete project jegan
```

Expected output
![image](https://github.com/user-attachments/assets/a5f49ee6-b408-4714-b4b0-a41ade8ff81c)

##
