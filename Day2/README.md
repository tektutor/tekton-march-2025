# Day 2

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

