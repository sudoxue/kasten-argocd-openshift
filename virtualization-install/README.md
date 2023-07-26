## Installing Openshift Virtualization Operator

OpenShift Virtualization extends Red Hat OpenShift Container Platform, allowing you to host and manage virtualized workloads on the same platform as container-based workloads. 

**Pre-requisites**: 
If you haven't setup the Argo CD on the openshift cluster yet, follow the instructions [here](https://github.com/smohandass/kasten-argocd-openshift/blob/main/README.md)


Create an ArgoCD application using the following yaml to install the OpenShift Virtualization operator and deployment.

```
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  finalizers:
  - resources-finalizer.argocd.argoproj.io
  name: virtualization-install
  namespace: openshift-gitops
spec:
  destination:
    namespace: openshift-cnv
    server: https://kubernetes.default.svc
  project: default
  source:
    directory:
      jsonnet: {}
      recurse: true
    path: virtualization-install
    repoURL: https://github.com/smohandass/kasten-argocd-openshift.git
    targetRevision: main
  syncPolicy:
    managedNamespaceMetadata:
      labels:
        argocd.argoproj.io/managed-by: openshift-gitops
    automated:
      selfHeal: true
    syncOptions:
      - CreateNamespace=true
```

Verify the argoCD is successfully created and status shows synced. It might take a few minutes for the operator and deployment to be created and status to shows Synced.

```
oc get app virtualization-install -n openshift-gitops     
NAME                     SYNC STATUS   HEALTH STATUS
virtualization-install   Synced        Healthy
```

Verify that all openshift virtualization pods are successfully running 

```
oc get pods -n openshift-cnv
```

