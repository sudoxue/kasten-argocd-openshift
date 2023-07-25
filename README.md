# kasten-argocd-openshift
Argo CD is an open-source continuous delivery (CD) tool to automate the deplyment and management of applications on kuberenetes clusters. This project demonstrates on how to use argoCD to install Kasten's K10 and automate K10 actions on Openshift environment.

# Table of Contents

1. Prerequisites
2. Installing Kasten using ArgoCD application

## Prerequisites
Prior to applying the examples defined in this project, ArgoCD need to be setup on the cluster. ArgoCD can be installed on Openshift clusters using the "Red Hat OpenShift GitOps" certified operator. To install the operator

  - Log into Openshift Console as Administrator user and navigate to Operators -> OperatorHub.
  - Filter by keyword `Red Hat OpenShift GitOps`.
  - Click Install
  - Use the default selected values
  - Click Install again to install the operator

Once the "Red Hat OpenShift GitOps" operator is installed, 

  - In openshift console, navigate to Operators -> Installed Operators -> Red Hat OpenShift GitOps
  - In Details tab, Click `Create Instance` in Argo CD tile.
  - In the form view, use default values and click Create.

This will create an instance of Argo CD in the `openshift-gitops` namespace.

Applications in Argo CD can be managed either by using the Arco CD UI or by using the argocd cli. 

To install the argocd cli using the instructions from [here](https://argo-cd.readthedocs.io/en/stable/cli_installation/). 

To access the Argo CD UI, click the cube icon on the top right corner in openshift console and click `Cluster Argo CD`. The default user to login is `admin`. Password can be retrieved from the secret `openshift-gitops-cluster` using the following oc command.

`oc get secret/openshift-gitops-cluster -n openshift-gitops -o jsonpath='{.data.admin\.password}' | base64 -d`


## Installing Kasten

Installing Kasten with Token Authentication : [k10-install/token-auth/README.md](https://github.com/smohandass/kasten-argocd-openshift/blob/main/k10-install/token-auth/README.md)
