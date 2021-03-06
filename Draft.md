# Draft

## Test Configure ArgoCD Vault plugin

```
oc apply -f https://raw.githubusercontent.com/lcolagio/lab-gitops-ops/master/draft/argocd-vault-plugin/base/sa-vplugin.yaml
oc apply -f https://raw.githubusercontent.com/lcolagio/lab-gitops-ops/master/draft/argocd-vault-plugin/argocd-vault-plugin-cluster1.yaml

```


## test App Gitops

oc project openshift-gitops
oc delete application app-demo-foo1
oc delete application app-demo-bar1
oc delete project app-demo-foo
oc delete project app-demo-bar


### Create app-demo-foo and app-demo-bar project and bind openshift-gitops-argocd-application rolebinding

```

cat << EOF | oc apply -f -
apiVersion: v1
kind: Namespace
metadata:
  annotations:
    argocd.argoproj.io/sync-wave: "-2"   
  name: app-demo-foo
EOF

cat << EOF | oc apply -f -
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: app-demo-foo-role-binding
  namespace: app-demo-foo
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: admin
subjects:
- kind: ServiceAccount
  name: openshift-gitops-argocd-application-controller
  namespace: openshift-gitops
EOF

cat << EOF | oc apply -f -
apiVersion: v1
kind: Namespace
metadata:
  annotations:
    argocd.argoproj.io/sync-wave: "-2"   
  name: app-demo-bar
EOF

cat << EOF | oc apply -f -
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: app-demo-bar-role-binding
  namespace: app-demo-bar
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: admin
subjects:
- kind: ServiceAccount
  name: openshift-gitops-argocd-application-controller
  namespace: openshift-gitops
EOF


```

### Test argocd deployment whitout vault plugin

Just to test simple argocd app


```
cat << EOF | oc apply -f -
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: app-demo-foo1
  namespace: openshift-gitops
  labels:
    gitops: app-demo
spec:
  destination:
    name: ''
    namespace: app-demo-foo
    server: 'https://kubernetes.default.svc'
  source:
    path: applications/app-test-argocd
    repoURL: 'https://github.com/lcolagio/lab-vault-plugin'
    targetRevision: HEAD
  project: dev
  syncPolicy:
    automated:
      prune: true
      selfHeal: true
EOF

oc delete application app-foo-dev -n openshift-gitops

cat << EOF | oc apply -f -
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: app-demo-bar1
  namespace: openshift-gitops
  labels:
    gitops: app-demo
spec:
  destination:
    name: ''
    namespace: app-demo-bar
    server: 'https://kubernetes.default.svc'
  source:
    path: applications/app-test-argocd
    repoURL: 'https://github.com/lcolagio/lab-vault-plugin'
    targetRevision: HEAD
  project: ops
  syncPolicy:
    automated:
      prune: true
      selfHeal: true
EOF
```