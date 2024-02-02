# EO DataHub Platform ArgoCD Deployment

This is the deployment repo for the UK EO DataHub Platform. The deployment uses the ArgoCD GitOps framework.

## ArgoCD

The applications deployed by this repo are designed to be managed by the ArgoCD GitOps framework. This deployment follows the app of apps pattern, where there is one "root" app that points to many child apps.

One of the apps deployed is ArgoCD itself, so that ArgoCD manages itself. For this to work, ArgoCD must already be installed on the cluster, manually or otherwise, to which the root app is deployed. After that, ArgoCD will manage its own deployment.

The version of ArgoCD bootstrapped in the cluster must match that of the root app.

### App of Apps

The app of apps pattern is used to deploy many ArgoCD applications together. The root app, _app.yaml_, points to a target Git repository and revision containing the source of truth for the application deployment. The target repo is the deployment's own repository. An additional path points to more applications within the repository, which are then deployed as child applications of the root app.

### Bootstrap ArgoCD

ArgoCD must first be deployed to the cluster in such a way that it manages itself. To do so, apply the kustomization that contains the ArgoCD application that watches the `self` eodhp-argocd-deployment (this repository), which also contains ArgoCD install manifest (referenced in the apps/argocd/kustomization.yaml file).

```bash
kubectl apply -k apps/argocd
```

At this point ArgoCD will now be:

- Deployed to the cluster.
- Watching this repo for changes.
- Managing all of the apps in _apps/_, including its own deployment.

### ArgoCD UI

To access the ArgoCD UI, use:

```bash
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo
kubectl port-forward service/argocd-server -n argocd 8080:443
```

You should now be able to access the UI on http://127.0.0.1:8080. The default username is _admin_ and the password is output to stdout after the first command.

### ArgoCD CRDs

All ArgoCD Application CRD manifests should be deployed to the namespace ArgoCD, e.g. Any Application CRDs. Otherwise, they will not be picked up and implemented by ArgoCD.
