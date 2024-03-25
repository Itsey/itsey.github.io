## Kubernetes notes.

Get the Kuztomize filefrom xaspiratea

abcdefgh
1234567890
```
2345678
kubectl apply -k .
kubectl get deployments
kubectl describe pod rts-api-774bb4f966-bz8sr
kubectl get events 
kubectl create deployment manual-test --image=rts-api:1.2.3.4
kubectl delete deployments --all

kubectl create secret docker-registry <SECRET_NAME>
  --docker-server <REGISTRY_NAME>.azurecr.io
  --docker-email <YOUR_MAIL>
  --docker-username=<SERVICE_PRINCIPAL_ID>
  --docker-password <YOUR_PASSWORD>
```



## Minikube

Install minikube on the server
Copy a cache from an internet enabled machine to the server
look up .kube\config and copy the ca.crt and the client.crt and the client.key along with the config file
from the .minikube folder

take these locally and edit the config file in .\kube merge the two files togheter
put the crt and key file on your machine

kubectl config use-context minikube should let u use the new context

