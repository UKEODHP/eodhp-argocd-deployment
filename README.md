# EO DataHub Platform ArgoCD Deployment

This is the deployment repo for the UK EO DataHub Platform. The deployment uses the ArgoCD GitOps framework.

## ArgoCD

The applications deployed by this repo are designed to be managed by the ArgoCD GitOps framework. This deployment follows the app of apps pattern, where there is one "root" app that points to many child apps.

One of the apps deployed is ArgoCD itself, so that ArgoCD manages itself. See Boostrap ArgoCD.

### Apps

Apps are managed as an ArgoCD ApplicationSet from the eodhp/ directory. The ArgoCD deployment has its own Application manifest as it requires different sync options to avoid deleting ArgoCD from the cluster when the application is uninstalled.

### Bootstrap ArgoCD

ArgoCD must first be deployed to the cluster in such a way that it manages itself. To do so, apply the kustomization that contains the ArgoCD application that watches the `self` eodhp-argocd-deployment (this repository), which also contains ArgoCD install manifest (referenced in the apps/argocd/kustomization.yaml file).

```bash
kubectl apply -k apps/argocd        # install argocd
kubectl apply -k eodhp/envs/<env>   # install argocd applications
```

Ensure your kubectl context is set correctly before deploying the argocd applications.

At this point ArgoCD will now be:

- Deployed to the target cluster.
- Watching this repo for changes.
- Managing all of the apps in _apps/_, including its own deployment.

### Delete EODHP

To remove EODHP from the cluster run:

```bash
kubectl delete -k eodhp/envs/<env>
```

### ArgoCD UI

To access the ArgoCD UI, use:

```bash
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo
kubectl port-forward service/argocd-server -n argocd 8080:443
```

You should now be able to access the UI on http://127.0.0.1:8080. The default username is _admin_ and the password is output to stdout after the first command.

### ArgoCD CRDs

All ArgoCD Application CRD manifests should be deployed to the namespace ArgoCD, e.g. Any Application CRDs. Otherwise, they will not be picked up and implemented by ArgoCD.

## Cluster Prerequisites

The EO DataHub Platform deployment expects the following resources to be available on the cluster.

### Storage Classes

- "block-storage" : for local pod persistent storage.
- "file-storage" : for NFS like file system storage that can be shared between pods.

### Ingress Class

- "nginx" : The ingress-nginx (https://kubernetes.github.io/ingress-nginx) ingress controller.

### Secret Stores

- "secret-store" : A ClusterSecretStore from external-secrets helm chart (https://charts.external-secrets.io).

### Resource Catalogue

The resource catalogue depends on there being a service account named `s3-access` in the namespace `rc`. This must 
give read/write permissions for the resource catalogue's S3 bucket.
