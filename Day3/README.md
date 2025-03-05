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
