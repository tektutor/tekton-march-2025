# Day 6

## Lab - Deploying an application into openshift using declarative style
In case you wish to generate the declarative manifest files yourself
```
cd ~
mkdir declartive-scripts
cd declarative-scripts

oc create deployment nginx --image=bitnami/nginx:latest --replicas=3 -o yaml --dry-run=client

oc create deployment nginx --image=bitnami/nginx:latest --replicas=3 -o yaml --dry-run=client > nginx-deploy.yml
```

Update the nginx-deploy.yml as shown below
![image](https://github.com/user-attachments/assets/b3b8eaa1-6bcc-40a5-a0d1-c31631122144)

You can create the nginx deployment in declarative style as shown below
```
oc project jegan
oc create -f nginx-deploy.yml
oc get deploy,rs,po
```

Expected output
![image](https://github.com/user-attachments/assets/745e27c8-d24e-4bcc-a582-2fd1ce2f2fdb)

## Lab - Declaratively creating clusterip internal service
```
oc project jegan
oc get deployments

oc expose deploy/nginx --port=8080 --type=ClusterIP -o yaml --dry-run=client
oc expose deploy/nginx --port=8080 --type=ClusterIP -o yaml --dry-run=client > nginx-clusterip-service.yml

oc create -f nginx-clusterip-service.yml --save-config
oc get services
oc describe svc/nginx
```

Expected output
![image](https://github.com/user-attachments/assets/cd814c43-aca7-4d2a-9afc-a687fe365fbf)
![image](https://github.com/user-attachments/assets/a66406dc-1181-4286-bb05-9fe7743b2edd)

