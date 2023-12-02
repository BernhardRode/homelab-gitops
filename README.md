# Ebbos Homelab

# Install Ubuntu Server

- Standard installation

# Install Microk8s

```bash
sudo mkdir -p /var/snap/microk8s/common/
sudo cp microk8s/microk8s-config.yaml /var/snap/microk8s/common/.microk8s.yaml
sudo snap install microk8s --classic
microk8s enable dns
microk8s enable metallb 192.168.1.200-192.168.1.210
```

## MicroK8s with your existing kubectl

```bash
sudo microk8s kubectl config view --raw > $HOME/.kube/config
```

# Install ArgoCD

```bash
microk8s kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

## Initial Password

Username is "admin"

```bash
microk8s kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
```

## External Access

```bash
microk8s kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'
microk8s kubectl get svc -n argocd argocd-server -o jsonpath="{.status.loadBalancer.ingress[0].ip}"
```
