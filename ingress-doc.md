1. Choose an Ingress Controller
For EKS, the most commonly used Ingress controllers are:

AWS ALB Ingress Controller: Integrates with AWS Application Load Balancer.
NGINX Ingress Controller: A widely-used, general-purpose Ingress controller.
For this guide, I’ll cover setting up the NGINX Ingress Controller. If you prefer using the AWS ALB Ingress Controller, please let me know, and I can provide steps for that as well.

2. Install NGINX Ingress Controller
2.1. Create a Namespace

First, create a namespace for the Ingress controller:
```
kubectl create namespace ingress-nginx
```
2.2. Apply the NGINX Ingress Controller Manifest

The official NGINX Ingress Controller manifest can be applied directly. This will deploy the NGINX Ingress Controller along with necessary components:
```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/cloud/deploy.yaml
```
2.3. Verify Installation

Ensure that the Ingress controller pods are running:
```
kubectl get pods -n ingress-nginx
```
You should see several pods running, including the nginx-ingress-controller.

3. Configure an Ingress Resource
Create an Ingress resource to route traffic to your application. Here’s an example YAML file for an Ingress resource:

ingress.yaml
```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: example-ingress
  namespace: default
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  rules:
  - host: example.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: my-service
            port:
              number: 80
```

3.1. Apply the Ingress Resource

Replace example.com with your domain and my-service with the name of your Kubernetes Service. Apply the Ingress resource:
```
kubectl apply -f ingress.yaml
```
3.2. Update DNS

To route traffic to your application, update your DNS settings to point to the external IP of the NGINX Ingress Controller. Retrieve the external IP:
```
kubectl get services -n ingress-nginx
```
Look for the service named ingress-nginx-controller and note its external IP. Configure your DNS to point to this IP address for example.com.

4. Verify Ingress Setup
Once DNS is updated, you can test your setup by navigating to http://example.com in your web browser. It should route traffic to the specified backend service.

5. Additional Configuration
5.1. TLS/SSL Configuration

For HTTPS, you can configure TLS/SSL certificates:

Create a Secret for TLS Certificates:
```
kubectl create secret tls tls-secret --cert=/path/to/tls.crt --key=/path/to/tls.key -n default
```
Update Ingress Resource to Use TLS:
Modify your ingress.yaml to include TLS configuration:
```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: example-ingress
  namespace: default
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  tls:
  - hosts:
    - example.com
    secretName: tls-secret
  rules:
  - host: example.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: my-service
            port:
              number: 80
```
Apply the Updated Ingress Resource:

```
kubectl apply -f ingress.yaml
```
