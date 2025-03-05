# Day 3

## Info - Kubelet
<pre>
- is a container Agent that runs in every type of node in Kubernetes and Openshift
- Kubelet runs as a Service
- Kubelet communicates with the container runtime to pull images, create containers, etc.,
- it runs in master and worker nodes
- it is also responsible to configure DNS in Pods
- Kubelet monitors the health of containers running on the local node and it reports the status frequently to API Server like heart-beat ( periodic fashion )
</pre>

## Info - Kube-proxy
<pre>
- is a Pod that runs in every node ( master and worker nodes )  
- it provides load-balancing for group of Pods that belong to a particular Deployment
</pre>

## Info - kube-dns
<pre>
- is a Pod reponsible for resolving the Service name to respective IP address
- there would be one kube-dns Pod running on each node
</pre>

## Lab - Describe a service to find the pod endpoints
```
oc project jegan
oc scale deploy/nginx --replicas=0
oc get services
oc describe service/nginx
oc get endpoints

oc scale deploy/nginx --replicas=3
oc get services
oc describe service/nginx
oc get endpoints
```

Expected output
![image](https://github.com/user-attachments/assets/46251478-3e3d-4ccf-96c2-a62208ebee03)


## Lab - Creating a nginx deployment and accessing the web pages using pods IP addresses
```
oc delete project jegan

oc new-project jegan
oc create deployment nginx --image=bitnami/nginx:latest --replicas=3
```
Expected output
![image](https://github.com/user-attachments/assets/27195d22-525e-469c-a483-1e0897f3c1d7)
![image](https://github.com/user-attachments/assets/cbd86f1b-7a80-47f6-bf5a-30d4c7f83940)


Let's list the pods with their IP address and the node where they are running
```
oc get pods -o wide
```

In case, you are getting ImagePullBack error due to docker rate limit, you could workaround by editing the deployment. You need to replace ImagePullPolicy from 'Always' to 'IfNotPresent', save and exit
```
oc edit deploy/nginx 
```

Now you should be able to see the pods
```
oc get po -o wide
```

Expected output
![image](https://github.com/user-attachments/assets/87518ef4-61d5-41ba-9af4-5d1d29582aea)
![image](https://github.com/user-attachments/assets/c7d41310-b44b-4e4e-bb40-9f4aee7e798e)


The Pods are assigned with Private Ips, they are accessible only within the Openshift cluster, ie. from openshift nodes or from Pods running within the cluster. In order to access the Pod IP, let's create a test deployment
```
oc create deployment test --image=tektutor/spring-ms:1.0
oc get po
```

Let's get inside the test pod shell
```
oc rsh deploy/test
curl http://<nginx-pod-ip>:8080
curl http://10.128.2.96:8080
curl http://10.131.0.86:8080
curl http://10.129.3.33:8080
```

Expected output
![image](https://github.com/user-attachments/assets/849ebb9f-dee5-48f9-9950-1ac9b9852c14)
![image](https://github.com/user-attachments/assets/ba753203-3a52-4920-8823-633e9b891628)
![image](https://github.com/user-attachments/assets/867bb79c-cef1-44bb-a522-d3bf8dc30372)
![image](https://github.com/user-attachments/assets/988db639-071e-41df-9588-8f11e680a9f2)

#### Note
<pre>
- This testing approach works well for developers
- This is not an option for end-users or production environments
- For end-users, we need to create service with routes to provide an user-friendly public url 
</pre>
