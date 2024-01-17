# Example: Guestbook application on Kubernetes

This directory contains the source code and Kubernetes manifests for PHP
Guestbook application.

Follow the tutorial at https://kubernetes.io/docs/tutorials/stateless-application/guestbook/.


minikube proxy
kubectl proxy --address='0.0.0.0' --port=8001 --accept-hosts='.*'
kubectl port-forward svc/guestbook-frontend 8086:80 --address 0.0.0.0 -n guestbook-prod
kubectl port-forward svc/argocd-server -n argocd 8080:443 --address 0.0.0.0