# Lab GitOps for Ops

## Add context to add a cluster to argocd

### Add cluster OCP

```
export CONTEXT=demo1
export CLUSTER=cluster-${GUID}
export DOMAIN=sandbox392.opentlc.com

oc login -u admin -p admin --insecure-skip-tls-verify https://api.${CLUSTER}.${DOMAIN}:6443
oc config rename-context $(oc config current-context) ${CONTEXT}
```

### Show context

```
oc config  get-contexts  | grep ${CONTEXT}
```

# switch context
```
oc config use-context ${CONTEXT}
oc config current-context && oc whoami
```



## Deploy Banner

You can choose color here: https://www.color-hex.com/

```
oc apply -k https://raw.githubusercontent.com/lcolagio/lab-gitops-ops/master/bootstrap/banner-demo1.yaml
```

