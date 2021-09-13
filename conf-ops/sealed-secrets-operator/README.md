source

# Bitnami Sealed Secrets

**Version: 0.15.0**

SealedSecret status stuck "Progressing" even when unsealed successfully #5991 

https://github.com/argoproj/argo-cd/issues/5991

## Add value 1 to SEALED_SECRETS_UPDATE_STATUS

```
kind: Deployment
metadata:
  name: sealed-secret-controller-sealed-secret
spec:
  template:
    spec:
      containers:
        - resources: {}
          env:
            - name: SEALED_SECRETS_UPDATE_STATUS
              value: '1'
```
              

