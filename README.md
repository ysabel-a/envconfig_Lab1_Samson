# envconfig_Lab1_Samson
FECP4-1022 Lab 1: Deploying environments in Kubernetes

# CHALLENGE:
---

1. Create production namespace
```
kubectl create namespace production
```
---
2. Create production configmap
```
kubectl create configmap app-config \
  --from-literal=APP_ENV=production \
  --namespace=production
```
---
3. Create production secrets
```
kubectl create secret generic app-secret \
  --from-literal=DB_PASSWORD=prodpassword123 \
  --namespace=production
```
---
4. Create production-deployment.yaml with 3 replicas using the app-deployment-template.yaml
```
sed -e 's/PLACEHOLDER_NAMESPACE/production/' \
    -e 's/replicas: .*$/replicas: 3/' app-deployment-template.yaml > production-deployment.yaml
```
---
5. Deploy
```
kubectl apply -f production-deployment.yaml
```
---
6. Expose production environment
```
kubectl expose production nginx-app \
  --type=NodePort \
  --name=nginx-service \
  --port=80 \
  --target-port=80 \
  --namespace=production
```
---
7. Verify deployment
```
kubectl get pods -n production
kubectl get service -n production
```
---
8. Access deployment
```
kubectl exec -it <pod-name> -n production -- /bin/bash
echo $APP_ENV
echo $DB_PASSWORD
```
