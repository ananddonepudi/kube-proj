az login  # Login to Azure
az group create --name demoRG --location eastus
az aks create --resource-group demoRG --name myAKSCluster --node-count 2 --enable-addons monitoring --generate-ssh-keys
az aks get-credentials --resource-group demoRG --name myAKSCluster
kubectl get nodes
kubectl create deployment my-demoapp --image=anandskd/demoapp

kubectl apply -f clusterip-service.yaml
kubectl get svc
kubectl run curl-pod --image=curlimages/curl -it --rm -- bash
curl http://my-app-clusterip:80

kubectl apply -f nodeport-service.yaml
kubectl get svc
kubectl get nodes -o wide
curl http://<NODE_IP>:30007
portforward
kubectl port-forward svc/my-app-nodeport 8080:80
minikube service my-app-nodeport --url

kubectl apply -f loadbalancer-service.yaml
kubectl get svc my-app-loadbalancer
curl http://<EXTERNAL_IP>:80

az group delete --name demoRG --yes --no-wait