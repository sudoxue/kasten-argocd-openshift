## Installing K10 with token authentication on Openshift

**Pre-requisites**: 
If you haven't setup the Argo CD on openshift cluster yet, follow the instructions [here](https://github.com/smohandass/kasten-argocd-openshift/blob/main/README.md)

Create the namespace where K10 will be installed on the openshift cluster and provide the Argo CD service account the admin role on the namespace. 

```
oc new-project kasten-io

oc adm policy add-role-to-user admin \
    system:serviceaccount:openshift-gitops:openshift-gitops-argocd-application-controller \
    -n kasten-io
```

Argo CD application can be created either using the argocd cli or using a yaml file. 

To install K10 using argoCD cli run the command
```
argocd app create k10-install \
      --app-namespace openshift-gitops \
      --dest-namespace kasten-io \
      --dest-server https://kubernetes.default.svc \
      --project default \
      --repo https://github.com/smohandass/kasten-argocd-openshift.git \
      --path k10-install/token-auth  \
      --revision main \
      --directory-recurse \
      --sync-policy automated \
      --self-heal
```

To install K10 using yaml use

```
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  finalizers:
  - resources-finalizer.argocd.argoproj.io
  name: k10-install
  namespace: openshift-gitops
spec:
  destination:
    namespace: kasten-io
    server: https://kubernetes.default.svc
  project: default
  source:
    directory:
      jsonnet: {}
      recurse: true
    path: k10-install/token-auth
    repoURL: https://github.com/smohandass/kasten-argocd-openshift.git
    targetRevision: main
  syncPolicy:
    automated:
      selfHeal: true
```

