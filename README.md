# Install prometheus operator to cluster
```
wget https://raw.githubusercontent.com/prometheus-operator/prometheus-operator/main/bundle.yaml -O bundle.yaml

kustomize build . | kubectl create -f -

kubectl port-forward svc/prometheus-operated 9090:9090
```



## Removal
```
kustomize build . | kubectl delete -f -
```
