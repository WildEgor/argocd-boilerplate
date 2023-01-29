# Usage

For EKS:

1) Create AWS Cluster:
```
    eksctl create cluster -f ./cluster/skeleton.yaml --auto-kubeconfig
```

and then use AWS EKS then set flag for eksctl or helm:
```
    --kubeconfig ./clusters/skeleton
```

2) Scale if needed:
```
    eksctl scale nodegroup --cluster=apps --nodes=4 --name=apps-nodes
```

1) Start minikube:
```
    minikube start --cpus "4" --disk-size "40000mb"
```
2) Deploy ArgoCD and apply current repository:
```
    kubectl create namespace argocd
    kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
    kubectl port-forward -n argocd svc/argocd-server 8080:443
    kubectl get secret argocd-initial-admin-secret -n argocd -o jsonpath="{.data.password}"
    (For win users: base64 -d -s <previous_result>)
    kubectl get secret argocd-initial-admin-secret -n argocd -o jsonpath="{.data.password}"
    argocd login localhost:8080 --username admin --password [password]
    (Optional: argocd account update-password --current-password <previous_pass> --new-password <new_pass>)
    argocd repo add https://github.com/WildEgor/argocd-boilerplate --username <github_username> --password <github_token>
    argocd app create apps --dest-namespace argocd --dest-server https://kubernetes.default.svc --repo https://github.com/WildEgor/argocd-boilerplate --path apps --revision develop
```
3) Install updater:
```
    helm repo add argo https://argoproj.github.io/argo-helm
    helm upgrade argocd-image-updater argo/argocd-image-updater -n argocd --values .\argo\values\updater.yaml --install --debug
```

# Hint
```
    Create new namespace
    kubectl create namespace [namespace]
    
    Use namespace instead of default
    kubectl config set-context --current --namespace=[namespace]
    
    Delete namespace and all resources
    kubectl delete namespace [namespace]
    
    Install app
    helm install [app-name] --dry-run --debug
    
    Show helms
    helm list --namespace [namespace]
    
    Unistall resource
    helm uninstall [release]
    
    Show resoures
    kubectl get all -n [namespace]
    
    Get logs 
    kubectl logs -n qabat -f [resourse-name]
    
    Get pods
    kubectl get pods -n [namespace]
    
    Get services
    kubectl get services -n [namespace]
    
    Get pod info
    kubectl describe pods -n [namespace] [pod-name]
    
    Show history
    helm history -n [namespace]
    
    Port forward
    kubectl port-forward [pod-name] <dest>:<source>
    
    kubectl config get-contexts
    
    kubectl config use-context minikube
```
