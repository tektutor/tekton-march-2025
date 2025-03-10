# Day 6

## Lab - Deploying an application into openshift using declarative style
In case you wish to generate the declarative manifest files yourself
```
cd ~
mkdir declarative-scripts
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

## Lab - Declaratively create nodeport external service

We need to delete the clusterip internal service
```
oc delete svc/nginx
```

Let's create the nodeport service in declarative style(i.e using yaml file)
```
oc get deployments
oc expose deploy/nginx --port=8080 --type=NodePort -o yaml --dry-run=client
oc expose deploy/nginx --port=8080 --type=NodePort -o yaml --dry-run=client > nginx-nodeport-service.yml
oc create -f nginx-nodeport-service.yml --save-config
oc get services
oc describe service/nginx
curl http://192.168.100.11:31673
```

Expected output
![image](https://github.com/user-attachments/assets/70142571-6163-4677-9d70-5fde33383d5b)
![image](https://github.com/user-attachments/assets/35d4a84d-3358-46e3-9ad1-df69a11047b1)
![image](https://github.com/user-attachments/assets/29dca678-7e46-48f2-b10c-367ace7ea2d6)


## Lab - Creating a LoadBalancer service for nginx deployment in declarative style using yaml 
Let's delete the nodeport service in declarative style
```
oc project jegan
oc delete -f nginx-nodeport-service.yml
```

Let's create a loadbalancer service for nginx deployment
```
oc project jegan
oc get deployments
oc expose deploy/nginx --type=LoadBalancer --port=8080 -o yaml --dry-run=client > nginx-lb-service.yml
oc create -f nginx-lb-service.yml
oc get svc
curl http://192.168.100.25:8080
```

Expected output
![image](https://github.com/user-attachments/assets/d37e8bf8-b72d-420c-8092-0924ee01d49c)
![image](https://github.com/user-attachments/assets/cc039e5a-ed3e-408b-886e-d0ba5b5632c3)


## Lab - Understanding rolling update ( upgrading live applcation from one version to other without downtime )
```
cat nginx-deploy.yml
oc create -f nginx-deploy.yml
oc get pods
oc get pods -o yaml | grep 1.26
```

Expected output
![image](https://github.com/user-attachments/assets/91bea36f-9953-4b1d-91e7-988f318a12f6)
![image](https://github.com/user-attachments/assets/1f3cdda7-18cb-44bb-9392-151438fcf6f7)
![image](https://github.com/user-attachments/assets/4607eaa8-3627-40f3-a311-92c3491bb43c)
![image](https://github.com/user-attachments/assets/2723bab1-ac11-438a-bca1-ab97227451d8)

Let's upgrade the nginx image from 1.26 to 1.27 as shown below
```
vim nginx-deploy.yml
cat nginx-deploy.yml
oc get rs
oc apply -f nginx-deploy.yml
oc get rs,po
oc rollout status deploy/nginx
oc get po -o yaml | grep 1.27
```

Expected output
![image](https://github.com/user-attachments/assets/fc7fdcdd-0449-473f-b9a9-f8f45a6d3dd7)
![image](https://github.com/user-attachments/assets/7ff5a092-6d2f-4939-b8e3-abbc4caeb6b6)
![image](https://github.com/user-attachments/assets/d3d96089-e9eb-4b50-b692-d5250172cb05)

Rollback due to some issue with the recently deployment version of your application
```
oc get po -o yaml | grep image
oc rollout undo deploy/nginx
oc rollout status deploy/nginx
oc get po -o yaml | grep image
oc rollout history deploy/nginx
```

Expected output
![image](https://github.com/user-attachments/assets/e847522c-ba23-4db7-9789-5754461c8f07)
![image](https://github.com/user-attachments/assets/1715adb9-5d83-4a0c-83bc-cb6babf3920a)


## Info - Istio Service Mesh
<pre>
- For sophisticated deployment strategy like blue-green, canary you could use Istio Service Mesh
- Istio Service Mesh also supports network policies
</pre>

## Info - Openshift Controllers
<pre>
- Controller manage resources in Kubernetes & Openshift
- each controller monitors and manages one type of Resource
- For example
  - Deployment Controller manages Deployment resource
  - ReplicaSet controller manages ReplicaSet resource
- Kubernetes/Openshift allows us to extend the Kubernetes/Openshift features(REST API) by definining a Custom Resource Definition (CRD)
- To manage your custom resource, you also must supply a Custom Controller
- Many Custom Resources and many Custom Controller can packaged as Operators and deployed into Openshift
- i.e we can add new functionalities to Openshift using Openshift Operators
- Kubernetes Operator Framework can be used to develop your own custom Operators using Golang, Ansible or Helm package manager
- Operators can be developed using
  - Golang programming language as Kubernetes/Openshift is implemented in Golang
  - Ansible we could develop Openshift operators
  - Helm package manager also supports some limited set of custom operators
</pre>

## Lab - Creating a Custom resource called Training using CRD

Create a yaml file named training-crd.yml
```
apiVersion: apiextensions.k8s.io/v1
kind: CustomResourceDefinition
metadata:
  name: trainings.tektutor.org 
spec:
  group: tektutor.org 
  scope: Namespaced
  names:
    kind: Training 
    listKind: TrainingList
    plural: trainings 
    singular: training 
    shortNames:
    - train 

  versions:
    - name: v1
      served: true
      storage: true 
      schema:
        openAPIV3Schema:
          type: object
          properties:
              training:
                type: string
              duration:
                type: string
              city:
                type: string
              from:
                type: string
              to:
                type: string
```

Now you can run the below command
```
oc get trainings
oc get training
oc get train
```

Let's create the Custom Resource called Training in Openshift
```
oc create -f training-crd.yml  --save-config

oc get trainings
oc get training
oc get train
```

Expected output


Let's create devops-training.yml as shown below
```
apiVersion: tektutor.org/v1
kind: Training 
metadata:
  name: devops-training 
spec:
  training: "Advanced DevOps"
  duration: "5 Days" 
  city: "Bengaluru"
  from: "4th July 2025"
  to: "8th July 2025"
```

Let's create the devops training
```
oc create -f devops-training.yml --save-config
```

Let's create openshift-training as shown below
```

apiVersion: tektutor.org/v1
kind: Training 
metadata:
  name: devops-training 
spec:
  training: "Advanced Tekton"
  duration: "5 Days" 
  city: "Bengaluru"
  from: "4th July 2025"
  to: "8th July 2025"
```

Let's run the below command
```
oc create -f openshift-training.yml --save-config
```

Listing the trainings
```
oc get trainings
oc get training
oc get train
oc describe train/devops-training
```

## Demo - Installing Tekton Operator in Openshift ( Only a Red Hat Openshift Administrator can do this one time activity )
Navigate to Red Hat Openshift Web console, Administrator context
![image](https://github.com/user-attachments/assets/4aec75f5-b3c0-4fb0-abd1-f35ff8239bb0)
Navigate to Operators --> Operator Hub
![image](https://github.com/user-attachments/assets/84331f1f-3c1d-4a0f-a586-471a49b0ada3)
Search for "TekTon"
![image](https://github.com/user-attachments/assets/781b6bf3-b194-4d9b-b00f-17b6ed8a6bbe)
Select "Red Hat Openshift Pipelines"
![image](https://github.com/user-attachments/assets/a4d6135b-bb0f-4cbe-9f1f-8a286d56ccf6)
Click "Install"
![image](https://github.com/user-attachments/assets/3da88d1a-4f67-469a-b618-1757df89b924)
Accept all the defaults
![image](https://github.com/user-attachments/assets/c049e692-2787-40ac-92b1-051aed0c5ec3)
![image](https://github.com/user-attachments/assets/294b9afa-b437-42e9-9011-c6922270f00a)
Click "Install"
![image](https://github.com/user-attachments/assets/11c8a11c-5056-4d26-8544-9e7c42735b51)
![image](https://github.com/user-attachments/assets/2b46429c-750f-471e-a0fb-0c0e18240b66)

The Red Hat Openshift Pipelines Operators adds the below TekTon Custom Resources
```
oc get crds | grep tekton
```
Expected output
![image](https://github.com/user-attachments/assets/b50fea55-cc0e-4622-a0b0-92324ca60357)

## Lab - Deploying an application from Openshift Webconsole in Developer login
Navigate to Red Hat Openshift webconsole as a Developer
![image](https://github.com/user-attachments/assets/284e39c0-9671-41ff-90fe-1dfbdb712d66)
Click +Add
![image](https://github.com/user-attachments/assets/f92156bd-2d82-40d2-b12f-1fa58fc53534)
Under the "Get started resources", click "View all samples"
![image](https://github.com/user-attachments/assets/43c3d5ed-82f0-434f-b760-0bbd39ec56f5)
![image](https://github.com/user-attachments/assets/61e54a41-d805-40a0-b4b0-7a23d49b4b24)
Select "Basic Nodejs"
![image](https://github.com/user-attachments/assets/2e43ed2a-04e5-415b-9f7e-0b926dfab8be)
Wait until the "Create" button gets enabled
![image](https://github.com/user-attachments/assets/d5ead3f4-c4b6-4557-a6ad-b0d86ebde76b)
![image](https://github.com/user-attachments/assets/28ea382d-e79a-466f-8865-0040e772dd0b)
Click "Create" button
![image](https://github.com/user-attachments/assets/fcee94c0-d3b8-44db-944b-257ac86013ca)
Build running
![image](https://github.com/user-attachments/assets/0ab3b7e7-98f3-4f64-9184-313e31210ebb)
![image](https://github.com/user-attachments/assets/7342541b-cc36-497d-bf45-c18f87f59d2a)
![image](https://github.com/user-attachments/assets/9722089a-7dd2-40ea-8efb-953ebc89514b)
![image](https://github.com/user-attachments/assets/45b149c6-9ab8-465d-8158-6442b115a556)
![image](https://github.com/user-attachments/assets/3b81302c-1330-41ee-95e3-f29b871203dd)
![image](https://github.com/user-attachments/assets/cab9a867-fb84-4f72-a2e6-cf99a2af748c)
![image](https://github.com/user-attachments/assets/50c80ab4-f697-4734-825a-4a98817db66f)

## Lab - Deploying sample application from Developer Catalog ( multi-pod application deployment )
![image](https://github.com/user-attachments/assets/76f3760a-fe8c-4935-a509-884c2b5ad215)
Click +Add
Select "Developer Catalog" --> All Services
![image](https://github.com/user-attachments/assets/322b0779-2a4d-49a2-8774-7fac00c53fc1)
Scroll down, Select "Django + Postgres (Ephemeral)" Template
![image](https://github.com/user-attachments/assets/306da52c-4fac-4e9a-b602-14664effa475)
Click "Instantiate Template"
![image](https://github.com/user-attachments/assets/3d5d8dab-50f1-416e-986c-09c7fd5c530a)
Accept all defaults and scroll down
![image](https://github.com/user-attachments/assets/ff3ea35d-7ad1-4d78-84ce-184a6246d335)
Click "Create" button
![image](https://github.com/user-attachments/assets/3b57744b-eed4-40e1-98e1-795a61ed3f64)
Build Progress
![image](https://github.com/user-attachments/assets/3738a69c-989d-4a89-9b93-5a6c813760b7)
![image](https://github.com/user-attachments/assets/b5b7921a-53f7-45f4-b9c5-74a9df78dbfb)
![image](https://github.com/user-attachments/assets/4543de7c-eb80-437c-827a-92ccd1e8367e)
![image](https://github.com/user-attachments/assets/ad060a37-e52f-42a6-9010-f2da52b249fa)
![image](https://github.com/user-attachments/assets/e8588192-b67b-4c42-9912-7adcf428f2bc)

