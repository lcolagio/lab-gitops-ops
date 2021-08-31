# Lab GitOps for Ops


## Deploy OpenShift GitOps (ArgoCD)

```
oc apply -k https://github.com/lcolagio/lab-gitops-ops/conf-ops/openshift-gitops-operator/overlays/stable
```
or
```
git clone https://github.com/lcolagio/lab-gitops-ops
oc apply -k lab-gitops-ops/conf-ops/openshift-gitops-operator/overlays/stable
```


## Add managed OCP cluster to ArgoCD

### Create context

```
CONTEXT=demo1
CLUSTER=cluster-${GUID}
DOMAIN=sandbox392.opentlc.com
KUBEADMIN_PWD=xxxx

oc login -u kubeadmin -p ${KUBEADMIN_PWD} --insecure-skip-tls-verify https://api.${CLUSTER}.${DOMAIN}:6443
oc config rename-context $(oc config current-context) ${CONTEXT}
```

### Show context

```
oc config  get-contexts  | grep ${CONTEXT}
```

### switch context
```
oc config use-context ${CONTEXT}
oc config current-context && oc whoami
```

### login to OpenShift Gitops (ArgoCD)
ARGO_PWD=$(oc -n openshift-gitops get secret openshift-gitops-cluster -o jsonpath='{.data.admin\.password}' | base64 -d)
ARGO_ROUTE=$(oc get route openshift-gitops-server -o jsonpath='{.spec.host}' -n openshift-gitops)
argocd --insecure --grpc-web login $ARGO_ROUTE:443  --username admin --password $ARGO_PWD

### Add Cluster to ArgoCD
argocd --insecure --grpc-web cluster add ${CONTEXT}


## Add cluster configuration


### Deploy Banner

You can choose color here: https://www.color-hex.com/

```
oc apply -f https://raw.githubusercontent.com/lcolagio/lab-gitops-ops/master/bootstrap/banner-demo1.yaml
```

### ...

## ...

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
git pull && git add . && git commit -m "xxx" && git push
```
