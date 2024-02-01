# EO DataHub Platform ArgoCD Deployment

This is the deployment repo for the UK EO DataHub Platform. The deployment uses the ArgoCD GitOps framework.

## ArgoCD

The applications deployed by this repo are designed to be managed by the ArgoCD GitOps framework. This deployment follows the app of apps pattern, where there is one "root" app that points to many child apps.

One of the apps deployed is ArgoCD itself, so that ArgoCD manages itself. For this to work, ArgoCD must already be installed on the cluster, manually or otherwise, to which the root app is deployed. After that, ArgoCD will manage its own deployment.

The version of ArgoCD bootstrapped in the cluster must match that of the root app.

### App of Apps

The app of apps pattern is used to deploy many ArgoCD applications together. The root app, _app.yaml_, points to a target Git repository and revision containing the source of truth for the application deployment. The target repo is the deployment's own repository. An additional path points to more applications within the repository, which are then deployed as child applications of the root app.

### Bootstrap ArgoCD

In the ArgoCD deployment, ArgoCD is deployed via Helm chart. To bootstrap the deployment it must initially be deployed to the cluster, so that it may manage its own deployment. This initial bootstrapping can be done manually, or it can be automated as part of the cluster setup.

To install ArgoCD to the cluster manually:

```bash
helm repo add argocd https://argoproj.github.io/argo-helm
helm install argocd argocd/argo-cd --version VERSION
```

Ensure that VERSION matches the version stated in the relevant _apps/\*\*/argocd/argocd.yaml_ `spec.source.targetRevision` for your deployment environment.

Once you have installed ArgoCD to the cluster you can then apply the root app manifest to the cluster:

```bash
kubectl apply -f app.yaml
```

At this point ArgoCD will now be managing itself.

### ArgoCD CRDs

All ArgoCD Application CRD manifests should be deployed to the namespace ArgoCD, e.g. Any Application CRDs. Otherwise, they will not be picked up and implemented by ArgoCD.
