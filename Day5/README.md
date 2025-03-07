# Day 5

## Demo - Importing docker image from Docker Hub Repository to Openshift's Internal registry
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
