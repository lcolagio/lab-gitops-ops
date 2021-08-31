# Lab GitOps for Ops

## Add context to add a cluster to argocd

### Add cluster OCP

```
CONTEXT=demo1
CLUSTER=cluster-${GUID}
DOMAIN=sandbox392.opentlc.com

oc login -u admin -p admin --insecure-skip-tls-verify https://api.${CLUSTER}.${DOMAIN}:6443
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


## Deploy Banner

You can choose color here: https://www.color-hex.com/

```
oc apply -f https://raw.githubusercontent.com/lcolagio/lab-gitops-ops/master/bootstrap/banner-demo1.yaml
```

# xxxx
