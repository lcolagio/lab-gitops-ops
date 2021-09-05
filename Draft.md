## Draft Configuration

### Configure ArgoCD Vault plugin

```
oc apply -f https://raw.githubusercontent.com/lcolagio/lab-gitops-ops/master/draft/argocd-vault-plugin/base/sa-vplugin.yaml
oc apply -f https://raw.githubusercontent.com/lcolagio/lab-gitops-ops/master/draft/argocd-vault-plugin/argocd-vault-plugin-cluster1.yaml

```

### Create vplugin-demo project with openshift-gitops-argocd-application rolebinding

```
oc new-project vplugin-demo

cat << EOF | oc apply -f -
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: vplugin-demo-role-binding
  namespace: vplugin-demo
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
oc delete application app-foo-dev -n openshift-gitops

cat << EOF | oc apply -f -
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: app-foo-dev
  namespace: openshift-gitops
  labels:
    gitops: app-foo
spec:
  destination:
    name: ''
    namespace: vplugin-demo
    server: 'https://kubernetes.default.svc'
  source:
    path: applications/app-test-argocd
    repoURL: 'https://github.com/lcolagio/lab-vault-plugin'
    targetRevision: HEAD
  project: dev
EOF
```


Just to test simple argocd app

```
oc delete application app-foo-dev -n openshift-gitops

cat << EOF | oc apply -f -
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: app-foo-dev
  namespace: openshift-gitops
  labels:
    gitops: app-foo
spec:
  destination:
    name: ''
    namespace: vplugin-demo
    server: 'https://kubernetes.default.svc'
  source:
    path: applications/app-test-argocd
    repoURL: 'https://github.com/lcolagio/lab-vault-plugin'
    targetRevision: HEAD
  project: ops
EOF
```