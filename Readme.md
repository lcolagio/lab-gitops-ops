# Lab GitOps for Ops


## Deploy OpenShift GitOps (ArgoCD) to OCP Hub Cluster


### Instal OpenShift GitOps  
```
oc apply -k https://github.com/lcolagio/lab-gitops-ops/conf-ops/openshift-gitops-operator/overlays/stable
```


### Login to ArgoCD
```
ARGO_PWD=$(oc -n openshift-gitops get secret openshift-gitops-cluster -o jsonpath='{.data.admin\.password}' | base64 -d)
ARGO_ROUTE=$(oc get route openshift-gitops-server -o jsonpath='{.spec.host}' -n openshift-gitops)
argocd --insecure --grpc-web login $ARGO_ROUTE:443  --username admin --password $ARGO_PWD
echo ${ARGO_PWD}
```

### Add Git Repo to ArgoCD
```
argocd repo add https://github.com/lcolagio/lab-gitops-ops  --name git1
```


## Add managed OCP cluster to ArgoCD

### Create context

<!-- ```
CONTEXT=ocp-lab1
CLUSTER=ocp-lab1
DOMAIN=dev.redlabclub.eu
KUBEADMIN_PWD=xxxx

oc login -u kubeadmin -p ${KUBEADMIN_PWD} --insecure-skip-tls-verify https://api.${CLUSTER}.${DOMAIN}:6443
oc config rename-context $(oc config current-context) ${CONTEXT}
``` -->

```
CONTEXT=demo1
CLUSTER=cluster-${GUID}
DOMAIN=sandboXXX.opentlc.com
KUBEADMIN_PWD=XXXX

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


<!-- ```  ``` -->
<!-- ```  ``` -->
<!-- ```  ``` -->

## Add cluster configuration


### Configure Banner

You can choose color ;-)
https://www.color-hex.com/

```
oc apply -f https://raw.githubusercontent.com/lcolagio/lab-gitops-ops/master/bootstrap/banner-demo1.yaml
```

### Configure Oauth-htpass

```
oc apply -f https://raw.githubusercontent.com/lcolagio/lab-gitops-ops/master/bootstrap/oauth-htpass-demo1.yaml
```

### Configure Permissions

```
oc apply -f https://raw.githubusercontent.com/lcolagio/lab-gitops-ops/master/bootstrap/permissions-demo1.yaml
```

### Configure sealedSecret

```
oc apply -f https://raw.githubusercontent.com/lcolagio/lab-gitops-ops/master/bootstrap/sealed-secrets-operator-demo1.yaml
```

<!-- ```
oc apply -k https://github.com/lcolagio/lab-gitops-ops/conf-ops/sealed-secrets-operator/base
``` -->

### Configure Argocd

```
oc apply -f https://raw.githubusercontent.com/lcolagio/lab-gitops-ops/master/bootstrap/argocd-demo1.yaml
```

<!-- ```
kustomize build https://github.com/lcolagio/lab-gitops-ops/conf-ops/argocd/overlays/demo1
``` -->

## ...

### ...

## ...

### ...

## ...


### ...

## ...

## Draf Cconfiguration

### Configure ArgoCD Vault plugin

#### Configurer le DEX
```
...
```

#### Configurer le vault-plugin

```
oc apply -f https://raw.githubusercontent.com/lcolagio/lab-gitops-ops/master/draft/argocd-vault-plugin/base/sa-vplugin.yaml
oc apply -f https://raw.githubusercontent.com/lcolagio/lab-gitops-ops/master/draft/argocd-vault-plugin/argocd-vault-plugin-demo1.yaml

```

<!-- ```  ``` -->
<!-- ```  ``` -->
<!-- ```  ``` -->

## Annexes

### Connect to Repo Git

````
git clone https://github.com/lcolagio/lab-gitops-ops
cd lab-gitops-ops

git config --global user.name "lcolagio"
git config --global user.email "lcolagio@redhat.com"
````

Use git token ...
```
git pull
git add . && git commit -m "xxx" && git push
```

### Kubernetes Kustomize - JsonPatches6902 overview

https://skryvets.com/blog/2019/05/15/kubernetes-kustomize-json-patches-6902/