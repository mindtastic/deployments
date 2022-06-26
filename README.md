# deployments
This repository contains deployment manifests for mindtastic's services

# Usage

## Creating and deploying a new application

In order to deploy a new application to Kuberentes you will need to do the following two things:
1. Create an `Application` manifest in `./applications`;
    You can use the template below to create an application manifest for your application. 
    Give it an appropriate name and store it in `./application`.
2. create matching Kubernetes manifests for your application in a top level directory.
    Create a directory for your application manifests and save them.
    Make sure that your application manifest matches the directory name in the `.spec.source.path` attribute.

Once merged to the main branch, your new application should show up in ArgoCD shortly after.

Just press "Sync" to consolidate the manifests and cluster state and deploy your application.

### Application Manifest Template

Use the following template to create a new Application Manifest:

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: guestbook   # The name of your application
  namespace: argocd # Do not change this
  finalizers:
    - resources-finalizer.argocd.argoproj.io
spec:
  project: default
  source:
    repoURL: https://github.com/mindtastic/deployments
    targetRevision: HEAD
    path: guestbook # This points to the path where your application's manifests are stored
  destination:
    server: https://kubernetes.default.svc
    namespace: guestbook # Namespace for your application
```

# Bootstraping

A major chart version change indicates that there is an incompatible breaking change needing manual actions.

`./applications.yaml` deploys an ArgoCD application which creates all other applications stored in `./applications`.

When bootstrapping a new Cluster, `applications.yaml` needs to be applied manually. After that it automatically adds all new applications added without any need to sync `applications` again.