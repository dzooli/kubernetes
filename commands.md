# K3S commands 

```bash
# Get pods scheduled to a node
kubectl get pods -A --field-selector spec.nodeName=<NODE>

# Pause a deployment
kubectl scale deployment <DEPLOYMENT> --replicas=0

# Traefik dashboard
kubectl port-forward $(kubectl get pods --selector "app.kubernetes.io/name=traefik" --output=name -n kube-system) -n kube-system 9000:9000 # then localhost:9000/dashboard/

# Delete traefik helm
kubectl -n kube-system delete helmcharts.helm.cattle.io traefik-crd
kubectl -n kube-system delete helmcharts.helm.cattle.io traefik
```

