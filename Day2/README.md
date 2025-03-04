# Day 2

# Info - Control Plane Components ( runs in every master node )
<pre>
1. API Server (Pod)
2. etcd (Pod)
3. scheduler (Pod)
4. controller managers (Pod)
</pre>  

## Info - Controller Managers 
<pre>
- is a collectionof many controllers
- controller is an application that runs always like a server
- each controller manages one type of resource
- controller keeps detects when
  - new resources are created in the cluster
  - existing resources are modified in the cluster
  - existing resources are deleted from the cluster
- For example
  - Deployment Controller manages Deployment resource
  - ReplicaSet Controller manages ReplicaSet resource
- example
  - Deployment Controller
  - ReplicaSet Controller
  - Job controller
  - CronJob controller
  - StatefulSet controller
  - DaemonSet Controller
</pre>  

## Info - Pod
<pre>
- Pod is a group of related containers
- Applications run within a Pod
- Each Pod has atleast 2 containers in it
- Each Pod will have just one main application
- The other hidden pod is called infra-pod aka pause container
- The pause container supports networking for the application container
- in addition to these 2 containers, it is possible to have additional container within the same Pod as side-car containers
- side-car container are supporting containers that will help expose the application metrics, logs, etc for elk, splunk, prometheus, etc.,
- this is the smallest unit that can be deployed into Kubernetes or Openshift
- For example
  - Let's say we have a microservice that retrieve data from a mongodb database
  - In this case, the microservice application will be deployed in one Pod, while the mongodb database will be deployed onto another Pod
  - This way, we can scale up/down the microservice Pods independently without scaling up/down of mongodb pods
</pre>

## Info - ReplicaSet
<pre>
- is a configuration that tells how many desired pods are supposed to be running
- ReplicaSet Controller use this configuration to perform scale up/down 
- Replicaset tells how many Pod instances should be running at any point of time
- Desired number of Pods, Actual number of Pods
- Whenever there is a difference between the desired number of Pods and actual number of Pods, the Replication Controller will create additional in case the actual number of Pods are less than the desired number of Pods.
- In case the actual number of Pods is greater than the desired number of Pods, the Replication Controller will delete some Pods to match the desired and actual number of Pods
</pre>

## Info - Deployment
<pre>
- is a configuration that tells 
  - what is the name of the deployment(application)
  - how many Pod instances should be created
  - what is the container image that must be used to create the Pod(s)
- Deployment configuration is taken as an input by Deployment Controller
- Deployment Controller manages Deployment
- Deployment Controller keeps monitoring for
  - new Deployment created
  - existing Deployment edited/updated
  - existing deployment deleted
- Whenever resources
  - are created
  - edited
  - deleted
  api server will notify via events, controllers will then act to ensure the desired state and actual state matches
</pre>


## Lab - Listing the openshift cluster nodes 
```
oc get nodes
```

Expected output
![image](https://github.com/user-attachments/assets/febebd94-3d98-4a09-a6f5-235ae218c4e1)


## Lab - Finding more details about openshift node
```
oc describe node master01.ocp4.alchemy.com
oc describe node master02.ocp4.alchemy.com
oc describe node master03.ocp4.alchemy.com
oc describe node worker01.ocp4.alchemy.com
oc describe node worker02.ocp4.alchemy.com
oc describe node worker03.ocp4.alchemy.com
```

Expected output
![image](https://github.com/user-attachments/assets/4018c513-aa59-498e-8b94-10980e33490a)
![image](https://github.com/user-attachments/assets/6e31761b-e1c7-44d2-b435-9f13ad904136)
![image](https://github.com/user-attachments/assets/66bc2484-a7e3-4849-9448-1e41d18c4882)
![image](https://github.com/user-attachments/assets/c3588f8d-fd56-436c-bbc5-6309b4793bb2)

## Lab - Creating a new-project in Openshift
<pre>
- before deploying our application, we need to create a project aka namespace in Kubernetes
- within the project, we can deployment our applications
- one project can have multiple application deployments
- project must be all lower case only
</pre>

Let's create a deployment in Openshift using imperative command
```
oc new-project jegan
```
Find the currently active project
```
oc project
```

Expected output
![image](https://github.com/user-attachments/assets/7dc521aa-0f06-41fd-bb8a-0e348932c0a9)
