# Intro
* Prometheus collects kubernetes metrics
* Grafana visualizes

# Prereq
Kubernetes cluster, local test cluster with node-ip capabilities works (e.g. minikube)

# Install prometheus operator to cluster
```
# fetch latest prometheus-operator bundle
wget https://raw.githubusercontent.com/prometheus-operator/prometheus-operator/main/bundle.yaml -O prometheus_operator.yaml

kustomize build . | kubectl create -f -
```

## Check prometheus
```
kubectl port-forward svc/prometheus-operated 9090:9090 # optionally check prometheus
```

## Configure grafana
```
kubectl port-forward svc/grafana 3000:3000
```

* Open http://localhost:3000 in your browser to access your Grafana dashboard. 
* Click Data Sources, then select Prometheus and fill in your configuration details e.g. http://<node_ip>:30900 in URL box
    ```
    kubectl get nodes -o wide # get node_ip
    ```
* You can’t use http://localhost:9090 as your HTTP URL because Grafana won’t have access to it. You must expose prometheus using a NodePort or LoadBalancer.
* Add and configure a dashboard, export to json


## Removal
```
kustomize build . | kubectl delete -f -
```
