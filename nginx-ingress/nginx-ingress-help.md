# Ngnix Ingress controller

## Installation

```bash
$ helm install nginx-ingress-latest oci://registry-1.docker.io/bitnamicharts/nginx-ingress-controller

Pulled: registry-1.docker.io/bitnamicharts/nginx-ingress-controller:11.5.4
Digest: sha256:359506995de843c4602111bd4203ff3a66d17fc4a33bbaca7fa279f602203883
NAME: nginx-ingress-latest
LAST DEPLOYED: Sun Nov 17 02:30:39 2024
NAMESPACE: default
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
CHART NAME: nginx-ingress-controller
CHART VERSION: 11.5.4
APP VERSION: 1.11.3

** Please be patient while the chart is being deployed **

The nginx-ingress controller has been installed.

Get the application URL by running these commands:

 NOTE: It may take a few minutes for the LoadBalancer IP to be available.
        You can watch its status by running 'kubectl get --namespace default svc -w nginx-ingress-latest-nginx-ingress-controller'
```

## Usage

```bash
export SERVICE_IP=$(kubectl get svc --namespace default nginx-ingress-latest-nginx-ingress-controller -o jsonpath='{.status.loadBalancer.ingress[0].ip}')
echo "Visit http://${SERVICE_IP} to access your application via HTTP."
echo "Visit https://${SERVICE_IP} to access your application via HTTPS."
```

An example Ingress that makes use of the controller:

```yml
  apiVersion: networking.k8s.io/v1
  kind: Ingress
  metadata:
    name: example
    namespace: default
  spec:
    ingressClassName: nginx
    rules:
      - host: www.example.com
        http:
          paths:
            - backend:
                service:
                  name: example-service
                  port:
                    number: 80
              path: /
              pathType: Prefix
    # This section is only required if TLS is to be enabled for the Ingress
    tls:
        - hosts:
            - www.example.com
          secretName: example-tls
```

If TLS is enabled for the Ingress, a Secret containing the certificate and key must also be provided:

```yml  
  apiVersion: v1
  kind: Secret
  metadata:
    name: example-tls
    namespace: default
  data:
    tls.crt: <base64 encoded cert>
    tls.key: <base64 encoded key>
  type: kubernetes.io/tls
```

WARNING: There are "resources" sections in the chart not set. Using "resourcesPreset" is not recommended for production. For production installations, please set the following values according to your workload needs:

- defaultBackend.resources
- resources
- +info <https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/>
