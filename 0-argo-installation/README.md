# gitops-argocd-workshop

# Kubeconfig
ssh adminuser@workshop01.noop.zone sudo k0s kubeconfig admin > kubeconfig
export KUBECONFIG=kubeconfig
# ArgoCD
```sh
cat > values.yml <<EOF
server:
  ingress:
    enabled: true
    ingressClassName: nginx
    annotations:
      nginx.ingress.kubernetes.io/force-ssl-redirect: "true"
      nginx.ingress.kubernetes.io/backend-protocol: "HTTPS"
    hosts: 
      - argocd.workshop01.noop.zone
    paths:
      - /
    pathType: Prefix
    tls:
      - hosts:
          - argocd.workshop01.noop.zone
EOF
helm repo add argo https://argoproj.github.io/argo-helm
helm upgrade --install argo-cd argo/argo-cd --version 5.13.6 -f values.yml
kubectl -n default get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
```