# Master 1 Kubernetes
Conteneurisation et Orchestration

### 1)  Lancer un Cluster Kubernetes avec Docker Desktop
```console
docker-compose up
```
### Cert Manager installation
```console
helm repo add jetstack https://charts.jetstack.io
helm repo update
helm install cert-manager jetstack/cert-manager --namespace cert-manager --create-namespace --version v1.8.0 --set installCRDs=true

kubectl apply -f .\cert_manager_deployment.yml
```

### 2)  Installation de ingress controllor : 
```console
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.1.0/deploy/static/provider/cloud/deploy.yaml

helm install nginx-ingress ingress-nginx/ingress-nginx --namespace appscore-ingress --create-namespace --set controller.replicaCount=2 --set controller.defaultTLS.cert=""
```

### 3) Installation de mon application :

```console
kubectl apply -f .\appscore_deployment.yaml
```

Pour supprimer:

```console
kubectl delete -f .\appscore_deployment.yaml
```

### 4) Installation de Prometheus et Grafana :

```console
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
helm install prometheus prometheus-community/kube-prometheus-stack -n appscore
```

### 5) Access to Grafana :

```console
kubectl port-forward -n appscore deployment/prometheus-grafana 3000
```

### 6) Installation ELK :

```console
Helm repo add elastic https://Helm.elastic.co
Helm install elasticsearch --namespace=appscore elastic/elasticsearch --set replicas=1 --set minimumMasterNodes=1 --set clusterHealthCheckParams="wait_for_status=yellow&timeout=1s"
Helm install kibana --namespace=appscore  elastic/kibana
```

### 7) Access to Kibana :
```console
kubectl port-forward -n appscore deployment/kibana-kibana 5601
