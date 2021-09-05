# Oauth

SECRET=htpass-secret.yaml

# Chiffrement du secret via le controler kubeseal
cat ${SECRET}  | kubeseal --controller-namespace=sealed-secrets --controller-name=sealed-secret-controller-sealed-secrets -o yaml --scope strict > ${SECRET}-sealed.yaml

# ou Chiffrement du secret via le cert (pem) récupéré précédement
cat ${SECRET} | kubeseal -o yaml --cert $HOME/.bitnami/publickey.pem > ${SECRET}-sealed.yaml


