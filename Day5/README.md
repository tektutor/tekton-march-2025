# Day 5

## Demo - Importing docker image from Docker Hub Repository to Openshift's Internal registry
```
docker login

oc create secret generic docker-pullsecret --from-file=.dockerjsonfile=/root/.docker/config.json --type=kubernetes.io/dockerconfigjson

oc import-image bitnami/nginx:latest bitnami/nginx:latest docker.io/library/bitnami/nginx:latest --confirm

```

Expected output
![outpu](img1.png)
![outpu](img2.png)
![outpu](img3.png)
![outpu](img4.png)
