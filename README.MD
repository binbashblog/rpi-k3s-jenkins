# How to use

Pretty simple:

* Install k3s on a Raspberry Pi 4:
```bash
curl -sfL https://get.k3s.io | sh -
```

* then git clone this repo, cd into it and run:
```bash
sudo kubectl create -f jenkins-deployment.yaml
sudo kubectl create -f jenkins-ingress.yaml
sudo kubectl create -f jenkins-svc.yaml
sudo kubectl create -f pvc.yaml
```

* You can use the following commands:
```bash
sudo kubectl get po
sudo kubectl describe po
sudo kubectl logs jenkins-0
sudo kubectl exec -it jenkins-0 -- /bin/bash
```

* To delete the container, ingres, service and pvc
```bash
sudo kubectl delete -f jenkins-deployment.yaml
sudo kubectl delete -f jenkins-ingress.yaml
sudo kubectl delete -f jenkins-svc.yaml
sudo kubectl delete -f pvc.yaml
```

* To obtain the jenkins initial admin password:
```bash
sudo kubectl exec -it jenkins-0 -- /bin/bash
view /var/jenkins_home/secrets/initialAdminPassword
exit
```
