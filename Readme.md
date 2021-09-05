# Lab GitOps for Ops

## Top References
- https://github.com/redlabclub/openshift4-config


## Install OpenShift GitOps (ArgoCD) to OCP Hub cluster
Login to OCP Hub cluster

<!-- 
CONTEXT=hub1
CLUSTER=cluster-0602
DOMAIN=sandbox392.opentlc.com
KUBEADMIN_PWD=n5btW-IqnhG-jRj4c-rRnuq 
-->

```
CONTEXT=???
CLUSTER=cluster-???
DOMAIN=sandbox???.opentlc.com
KUBEADMIN_PWD=????-????-????-????

oc login -u kubeadmin -p ${KUBEADMIN_PWD} --insecure-skip-tls-verify https://api.${CLUSTER}.${DOMAIN}:6443
oc config rename-context $(oc config current-context) ${CONTEXT}
```

### Boostrap OpenShift GitOps Operator
Install OpenShift GitOps Operator
```
oc apply -k https://github.com/lcolagio/lab-gitops-ops/conf-ops/openshift-gitops-operator/overlays/
```

### Boostrap Sealed-secrets
Insttall sealed-secrets-operator
```
oc apply -k https://github.com/lcolagio/lab-gitops-ops/conf-ops/sealed-secrets-operator/overlays/
```

Use backuped Sealed-secrets PEM, CERT
```
curl https://raw.githubusercontent.com/lcolagio/lab-gitops-ops/master/conf-ops/sealed-secrets-operator/scripts/replace-sealed-secrets-secret.sh | bash
```

### Configure Argocd
Add Dex, Add GitRepo, Add Remopte Cluster ...
```
oc apply -f https://raw.githubusercontent.com/lcolagio/lab-gitops-ops/master/bootstrap/argocd-cluster1.yaml
```

<!-- ```
kustomize build https://github.com/lcolagio/lab-gitops-ops/conf-ops/argocd/overlays/cluster1
``` -->

### Configure sealedSecret
```
oc apply -f https://raw.githubusercontent.com/lcolagio/lab-gitops-ops/master/bootstrap/sealed-secrets-operator-cluster1.yaml
```
<!-- ```
oc apply -k https://github.com/lcolagio/lab-gitops-ops/conf-ops/sealed-secrets-operator/base
``` -->

<!-- ```  ``` -->
<!-- ```  ``` -->
<!-- ```  ``` -->

## Add cluster configuration


### Configure Banner
You can choose color ;-)
https://www.color-hex.com/

```
oc apply -f https://raw.githubusercontent.com/lcolagio/lab-gitops-ops/master/bootstrap/banner-cluster1.yaml
```

### Configure Oauth-htpass
```
oc apply -f https://raw.githubusercontent.com/lcolagio/lab-gitops-ops/master/bootstrap/oauth-htpass-cluster1.yaml
```

### Configure Permissions
```
oc apply -f https://raw.githubusercontent.com/lcolagio/lab-gitops-ops/master/bootstrap/permissions-cluster1.yaml
```


## ...

### ...

## ...

### ...

## ...

### ...

## ...

<!-- ```  ``` -->
<!-- ```  ``` -->
<!-- ```  ``` -->

# Annexes


## Install CLI ttols

```
### ArgoCD
wget https://github.com/argoproj/argo-cd/releases/download/v2.1.1/argocd-linux-amd64
mv argocd-linux-amd64 argocd
chmod +x argocd
sudo mv argocd /usr/bin/argocd
argocd version

### Kustomize
curl -s "https://raw.githubusercontent.com/kubernetes-sigs/kustomize/master/hack/install_kustomize.sh"  | bash
sudo mv kustomize /usr/bin/kustomize

### kubeseal
wget https://github.com/bitnami-labs/sealed-secrets/releases/download/v0.12.4/kubeseal-linux-amd64 -O kubeseal
sudo install -m 755 kubeseal /usr/local/bin/kubeseal
rm kubeseal

### Install helm
wget https://mirror.openshift.com/pub/openshift-v4/clients/helm/latest/helm-linux-amd64
mv helm-linux-amd64 helm
chmod +x helm
sudo mv helm /usr/bin/helm
helm version
```

## Login to ArgoCD
```
ARGO_PWD=$(oc -n openshift-gitops get secret openshift-gitops-cluster -o jsonpath='{.data.admin\.password}' | base64 -d)
ARGO_ROUTE=$(oc get route openshift-gitops-server -o jsonpath='{.spec.host}' -n openshift-gitops)
argocd --insecure --grpc-web login $ARGO_ROUTE:443  --username admin --password $ARGO_PWD
echo ${ARGO_PWD}
```

## Add Git Repo to ArgoCD
```
argocd repo add https://github.com/lcolagio/lab-gitops-ops  --name git1
```

## Add managed OCP cluster to ArgoCD

### Create context

```
CONTEXT=cluster1
CLUSTER=cluster-0602
DOMAIN=sandbox392.opentlc.com
KUBEADMIN_PWD=n5btW-IqnhG-jRj4c-rRnuq

oc login -u kubeadmin -p ${KUBEADMIN_PWD} --insecure-skip-tls-verify https://api.${CLUSTER}.${DOMAIN}:6443
oc config rename-context $(oc config current-context) ${CONTEXT}
```

### Show context

```
oc config  get-contexts  | grep ${CONTEXT}
```

### Switch context
```
oc config use-context ${CONTEXT}
oc config current-context && oc whoami
```

### Comeback to OCP hub cluster
```
oc config use-context admin
```

### Add managed cluster to ArgoCD
```
argocd --insecure --grpc-web cluster add ${CONTEXT}
```
