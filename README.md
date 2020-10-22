# OCP4demo
Testing new features on OCP4

# Install ArgoCD
```
oc new-project argocd
#The below step can be done installing the operator instead
oc apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

oc adm policy add-cluster-role-to-user cluster-admin -z argocd-application-controller -n argocd
oc -n argocd patch deployment argocd-server -p '{"spec":{"template":{"spec":{"$setElementOrder/containers":[{"name":"argocd-server"}],"containers":[{"command":["argocd-server","--insecure","--staticassets","/shared/app"],"name":"argocd-server"}]}}}}'
oc -n argocd create route edge argocd-server --service=argocd-server --port=http --insecure-policy=Redirect

#oc patch deployment argocd-dex-server  -p '{"spec": {"template": {"spec": {"containers": [{"name": "dex","image": "quay.io/redhat-cop/dex:v2.22.0-openshift"}]}}}}'
```

# Download ArgoCD CLI
```
VERSION=$(curl --silent "https://api.github.com/repos/argoproj/argo-cd/releases/latest" | grep '"tag_name"' | sed -E 's/.*"([^"]+)".*/\1/')
curl -sSL -o /usr/local/bin/argocd https://github.com/argoproj/argo-cd/releases/download/$VERSION/argocd-linux-amd64
sudo chmod +x /usr/local/bin/argocd
```

# Login to ArgoCD
```
ARGOCD_ROUTE=$(oc -n argocd get route argocd-server -o jsonpath='{.spec.host}')
ARGOCD_SERVER_PASSWORD=$(oc get secret argocd-cluster -n argocd -ojsonpath='{.data.admin\.password}' | base64 --decode)
argocd --insecure --grpc-web login ${ARGOCD_ROUTE}:443 --username admin --password ${ARGOCD_SERVER_PASSWORD}
```
