# Lab GitOps for Ops


## Deploy OpenShift GitOps (ArgoCD) to OCP Hub Cluster


### Instal OpenShift GitOps  
```
oc apply -k https://github.com/lcolagio/lab-gitops-ops/conf-ops/openshift-gitops-operator/overlays/stable
```

### Configure Argocd

Add Dex, GitRepo, Cluster ...
```
oc apply -f https://raw.githubusercontent.com/lcolagio/lab-gitops-ops/master/bootstrap/argocd-cluster1.yaml
```

<!-- ```
kustomize build https://github.com/lcolagio/lab-gitops-ops/conf-ops/argocd/overlays/cluster1
``` -->

<!-- ```  ``` -->
<!-- ```  ``` -->
<!-- ```  ``` -->


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

### Configure sealedSecret

```
oc apply -f https://raw.githubusercontent.com/lcolagio/lab-gitops-ops/master/bootstrap/sealed-secrets-operator-cluster1.yaml
```
<!-- ```
oc apply -k https://github.com/lcolagio/lab-gitops-ops/conf-ops/sealed-secrets-operator/base
``` -->

## ...

### ...

## ...

### ...

## ...

### ...

## ...

## Draft Configuration

### Configure ArgoCD Vault plugin

```
oc apply -f https://raw.githubusercontent.com/lcolagio/lab-gitops-ops/master/draft/argocd-vault-plugin/base/sa-vplugin.yaml
oc apply -f https://raw.githubusercontent.com/lcolagio/lab-gitops-ops/master/draft/argocd-vault-plugin/argocd-vault-plugin-cluster1.yaml

```

<!-- ```  ``` -->
<!-- ```  ``` -->
<!-- ```  ``` -->

# Annexes

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
