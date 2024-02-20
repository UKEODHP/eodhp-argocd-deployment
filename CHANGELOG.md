# Change Log

## v0.3.2

- Demonstrated importing secret in web-presence from secret-store

## v0.3.1

- Swapped AWS ALB for Ingress Nginx Controller due to limitations of AWS ALB routing (no rewrites)

## v0.3.0

- Added web presence app (content management system)

## v0.2.0

- Refactored directory structure and bootstrap procedure to fix ArgoCD stability issues
- Added EOxVS and removed the demo guestbook app

## v0.1.0

- Created the simplest self-managed ArgoCD deployment possible; single environment with two apps, ArgoCD itself and a demo Guestbook app. Guestbook app should be removed in subsequent releases.
