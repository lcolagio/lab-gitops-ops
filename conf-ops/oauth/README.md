# Oauth



# Create or uppdate htpassword

```
SECRET=htpass-secret-xxx

echo "Creating htpasswd file"

htpasswd -c -b -B htpasswd opentlc-mgr xxxx
htpasswd -b -B htpasswd admin xxxx

htpasswd -b -B htpasswd john xxxx
htpasswd -b -B htpasswd paul xxxx
htpasswd -b -B htpasswd ringo xxxx
htpasswd -b -B htpasswd george xxxx
htpasswd -b -B htpasswd pete xxxx

htpasswd -b -B htpasswd andrew xxxx
htpasswd -b -B htpasswd karla xxxx
htpasswd -b -B htpasswd marina xxxx

htpasswd -b -B htpasswd user1 xxxx
htpasswd -b -B htpasswd user2 xxxx
htpasswd -b -B htpasswd user3 xxxx

htpasswd -b -B htpasswd dev1 xxxx
htpasswd -b -B htpasswd dev2 xxxx
htpasswd -b -B htpasswd dev3 xxxx

htpasswd -b -B htpasswd ops1 xxxx
htpasswd -b -B htpasswd ops2 xxxx
htpasswd -b -B htpasswd ops3 xxxx

htpasswd -b -B htpasswd manager1 xxxx
htpasswd -b -B htpasswd manager2 xxxx
htpasswd -b -B htpasswd manager3 xxxx

htpasswd -b -B htpasswd dev xxxx
htpasswd -b -B htpasswd qua xxxx
htpasswd -b -B htpasswd prd xxxx


cat htpasswd | base64 -w 0

cat << EOF >${SECRET}.yaml

apiVersion: v1
kind: Secret
metadata:
  name: htpass-secret
  namespace: openshift-config
  annotations:
    argocd.argoproj.io/sync-options: Prune=false
    argocd.argoproj.io/compare-options: IgnoreExtraneous
data:
  htpasswd: $(cat htpasswd | base64 -w 0)
type: Opaque

EOF

# Chiffrement du secret via le controler kubeseal
cat ${SECRET}.yaml  | kubeseal --controller-namespace=sealed-secrets --controller-name=sealed-secret-controller-sealed-secrets -o yaml --scope strict > ${SECRET}-sealed.yaml

# ou Chiffrement du secret via le cert (pem) récupéré précédement
cat ${SECRET} | kubeseal -o yaml --cert $HOME/.bitnami/publickey.pem > ${SECRET}-sealed.yaml


