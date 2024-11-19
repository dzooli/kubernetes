# Use cert-manager.io's cert-manager with Nginx-ingress

## Installation

```bash
helm repo add jetstack https://charts.jetstack.io --force-update
helm install cert-manager jetstack/cert-manager --namespace cert-manager --create-namespace --version v1.16.1 --set crds.enabled=true
```

## Issuer creation

Use Issuer for a namespaced issuer or ClusterIssuer without namespace for a cluster-wide issuer.

```bash
kubectl create --edit -f https://raw.githubusercontent.com/cert-manager/website/master/content/docs/tutorials/acme/example/staging-issuer.yaml
kubectl create --edit -f https://raw.githubusercontent.com/cert-manager/website/master/content/docs/tutorials/acme/example/production-issuer.yaml
```

Staging issuer template:

```yaml
apiVersion: cert-manager.io/v1
kind: Issuer
metadata:
  name: letsencrypt-staging
spec:
  acme:
    # The ACME server URL
    server: https://acme-staging-v02.api.letsencrypt.org/directory
    # Email address used for ACME registration
    email: user@example.com
    # Name of a secret used to store the ACME account private key
    privateKeySecretRef:
      name: letsencrypt-staging
    # Enable the HTTP-01 challenge provider
    solvers:
      - http01:
          ingress:
            ingressClassName: nginx
```

Production issuer template:

```yaml
apiVersion: cert-manager.io/v1
kind: Issuer
metadata:
  name: letsencrypt-prod
spec:
  acme:
    # The ACME server URL
    server: https://acme-v02.api.letsencrypt.org/directory
    # Email address used for ACME registration
    email: user@example.com
    # Name of a secret used to store the ACME account private key
    privateKeySecretRef:
      name: letsencrypt-prod
    # Enable the HTTP-01 challenge provider
    solvers:
      - http01:
          ingress:
            ingressClassName: nginx
```

## Complete tutorial

<https://cert-manager.io/docs/tutorials/acme/nginx-ingress/#step-1---install-helm>
