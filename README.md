# Deploy-nodejs-mongodb-app-on-k8s
Deploying nodejs express backend application with MongoDB database on a kubernetes cluster.

# Prerequisite
kubernetes cluster runnig 
- In my case I used kubeadm to install kubernetes cluster in three ubuntu aws EC2
![App Screenshot](https://raw.githubusercontent.com/elmansey/Deploy-express-mongodb-app-on-k8s/main/Screenshot%20from%202023-03-26%2021-50-57.png)

![App Screenshot](https://raw.githubusercontent.com/elmansey/Deploy-express-mongodb-app-on-k8s/main/Screenshot%20from%202023-03-26%2015-54-58.png)

[Creating a cluster with kubeadm](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/create-cluster-kubeadm/)


# Steps
-  Create persistent volume and persistent volume claim to persist Mongodb data 
  ```bash
  kubectl apply -f persistent-volume.yaml
  kubectl apply -f mongodb-persistent-volume-claim.yaml
```
-  Create configMap and secret for MongoDB metadat and Credentials
  ```bash
  kubectl apply -f config.yaml
```
-  Create MongoDB Deployment and service 
  ```bash
  kubectl apply -f mongodb.yaml
```
-  Create Deployment for nodejs backend app 
  ```bash
  kubectl apply -f express-nodejs.yaml
```
-  Create service for nodejs backend app 
  ```bash
  kubectl apply -f node-app-svc.yaml
```

-  install Nginx Ingress Controller in Kubernetes by using helm to allow me apply ingress in cluster 
  ```bash
  helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
  helm repo update
  helm install ingress-nginx  ingress-nginx/ingress-nginx
```

-  Create external service (LoadBalancer)
![image](https://user-images.githubusercontent.com/89076648/227803023-cd048346-03d3-4838-8f23-97ccc4d2c683.png)

-  Create Ingress to make app accessible from outside cluster and connect it with loadBalancer
  ```bash
    kubectl apply -f ingress-class.yml
    kubectl apply -f express-node-app-ingress.yaml
```

-  Use the  LoadBalancer dns name  to access the service or hostname 

![image](https://user-images.githubusercontent.com/89076648/227803367-f232ce7c-1313-4f38-882e-fce6740553c8.png)





