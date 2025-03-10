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

