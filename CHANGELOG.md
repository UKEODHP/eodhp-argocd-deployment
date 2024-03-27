# Change Log

## v0.10.0

- Added Wagtail configuration environment variables

## v0.9.0

- Added notebook link from web presence

## v0.8.0

- Added job to add to resource catalogue S3 bucket from GitHub repo for Element84 data

## v0.7.0

- Added job to add to resource catalogue S3 bucket from GitHub repo

## v0.6.0

- Added STAC browser link from web presence

## v0.5.0

- Added Keycloak app (not integrated with anything)

## v0.4.0

- Updated repo directory structure to support multi environment deployments
- Swapped apps to use block-storage storage class

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
