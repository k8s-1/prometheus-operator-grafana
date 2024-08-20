# Install prometheus operator to cluster
wget https://raw.githubusercontent.com/prometheus-operator/prometheus-operator/main/bundle.yaml -O bundle.yaml
kubectl apply -f bundle.yaml --force-conflicts=true --server-side=true

## Removal
```
for n in $(kubectl get namespaces -o jsonpath={..metadata.name}); do
  kubectl delete --all --namespace=$n prometheus,servicemonitor,podmonitor,alertmanager
done

kubectl delete -f bundle.yaml

for n in $(kubectl get namespaces -o jsonpath={..metadata.name}); do
  kubectl delete --ignore-not-found --namespace=$n service prometheus-operated alertmanager-operated
done

kubectl delete --ignore-not-found customresourcedefinitions \
  prometheuses.monitoring.coreos.com \
  servicemonitors.monitoring.coreos.com \
  podmonitors.monitoring.coreos.com \
  alertmanagers.monitoring.coreos.com \
  prometheusrules.monitoring.coreos.com
```
kubectl apply -f prometheus_rbac.yaml
kubectl apply -f prometheus_instance.yaml
kubectl port-forward svc/prometheus-operated 9090:9090
