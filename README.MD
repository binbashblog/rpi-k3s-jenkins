# How to use

NOTE: Also tested to work on K8s as well as K3s

Pretty simple:

* Requires a NFS Server
Highly recommend to use an NFS Server instead of relying on the SD Card's reliability

* Also requires installing the nfs client on all your nodes

* JNLP (Applies to Windows agents using jnlp only, not agents that use websockets or the ssh launch method from the master)  

I have set the service port to TCP 50000 and the nodeport to TCP 30000.  
You need to configure any nodes external to the k8s cluster to tunnel to port 30000 or whichever port you decide to use.  
This is a workaround as the ingress only works on 80/443 http/https so you have to use a nodeport.  
Alternatively use a websocket or ssh on windows (requires ssh keypair).  
Create the node in Jenkins and enter the port 30000 under the Advanced option "Tunnel connection through".  
Port 30000 will then need to be forwarded from external node agents to the kubernetes external cluster ip which will then be forwarded to the container on port 50000...  


* Install k3s on a Raspberry Pi 4:
```bash
curl -sfL https://get.k3s.io | sh -
```

* Install the nfs client on all your nodes

Deb-based OS
```bash
apt-get install nfs-common
```
EL-based OS
```bash
yum install nfs-utils
```

* then git clone this repo, cd into it and run:
```bash
sudo kubectl create -f jenkins-namespace.yaml
sudo kubectl create -f jenkins-ingress.yaml
sudo kubectl create -f jenkins-svc.yaml
sudo kubectl create -f jenkins-pv.yaml
sudo kubectl create -f jenkins-pvc.yaml
sudo kubectl create -f jenkins-deployment.yaml
```

* You can use the following commands:
```bash
sudo kubectl get po
sudo kubectl describe po
sudo kubectl logs jenkins-0
sudo kubectl exec -it jenkins-0 -- /bin/bash
```

* To delete the container, ingres, service and pvc, etc basically a full cleanup
```bash
sudo kubectl delete -f jenkins-deployment.yaml
sudo kubectl delete -f jenkins-ingress.yaml
sudo kubectl delete -f jenkins-svc.yaml
sudo kubectl delete -f jenkins-pvc.yaml
sudo kubectl delete -f jenkins-pv.yaml
sudo kubectl delete -f jenkins-namespace.yaml
```

* To obtain the jenkins initial admin password:
```bash
sudo kubectl exec -it jenkins-0 -- /bin/bash
view /var/jenkins_home/secrets/initialAdminPassword
exit
```
