# Day 2

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
