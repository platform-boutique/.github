# platform-boutique

An open-source platform-engineering project: an online shop (ten polyglot microservices) built as the workload for a complete platform - GitOps, admission policy, observability, shared CI/CD, a developer portal and infrastructure as code for AWS (EKS). Everything here is public.

**Start here:** the [master reference and handoff](https://github.com/platform-boutique/platform-demo-docs) · the [roadmap board](https://github.com/orgs/platform-boutique/projects/1).

## Repositories by category

GitHub has no folders, so the category is in the **name prefix**, in the repository **custom property `category`** (filter: [`props.category:app`](https://github.com/orgs/platform-boutique/repositories?q=props.category%3Aapp), `deploy`, `platform`, `infra`, `docs`) and in the topics.

| Category | Prefix | Repositories |
|---|---|---|
| **app** - what we build, one repo per service | `app-` | [`app-frontend`](https://github.com/platform-boutique/app-frontend) (Go) · [`app-ad-service`](https://github.com/platform-boutique/app-ad-service) (Java) · [`app-cart-service`](https://github.com/platform-boutique/app-cart-service) (.NET) · [`app-checkout-service`](https://github.com/platform-boutique/app-checkout-service) (Go) · [`app-currency-service`](https://github.com/platform-boutique/app-currency-service) (Node) · [`app-email-service`](https://github.com/platform-boutique/app-email-service) (Python) · [`app-payment-service`](https://github.com/platform-boutique/app-payment-service) (Node) · [`app-product-catalog-service`](https://github.com/platform-boutique/app-product-catalog-service) (Go) · [`app-recommendation-service`](https://github.com/platform-boutique/app-recommendation-service) (Python) · [`app-shipping-service`](https://github.com/platform-boutique/app-shipping-service) (Go) · [`app-product-catalog-db`](https://github.com/platform-boutique/app-product-catalog-db) (PostgreSQL image of the shop's catalog database) |
| **deploy** - what runs where | `platform-demo-` | [`platform-demo-helm`](https://github.com/platform-boutique/platform-demo-helm): Helm charts, per-environment values, Argo CD config, Kyverno policies; Argo CD deploys it to the EKS cluster |
| **platform** - the machinery around the apps | `platform-demo-` | [`platform-demo-workflows`](https://github.com/platform-boutique/platform-demo-workflows): reusable CI/CD pipelines · [`platform-demo-observability`](https://github.com/platform-boutique/platform-demo-observability): Grafana LGTM stack (docker compose) and dashboards · [`platform-demo-backstage`](https://github.com/platform-boutique/platform-demo-backstage): the developer portal, with its plugins `platform-demo-backstage-plugin-{hubble,releases,policies,argocd}` |
| **infra** - clouds and clusters | `infra-` | [`infra-terraform`](https://github.com/platform-boutique/infra-terraform): the AWS platform (network, EKS, RDS, load balancer, DNS) and the bootstrap of Argo CD · *planned:* `infra-crossplane` (self-service infrastructure) |
| **docs** | `platform-demo-` | [`platform-demo-docs`](https://github.com/platform-boutique/platform-demo-docs): master reference, handoff, history |

*Planned under `platform`:* synthetic tests, monitors and alerts, AI tooling.

## Conventions

- **New service** -> `app-<name>` (+ one line in `platform-demo-helm/envs/<env>/workloads.yaml`). **New infrastructure code** -> `infra-<name>`. **New platform tooling** -> `platform-demo-<name>`. Set the `category` property when you create the repository.
- Trunk-based: short-lived branches, squash-merged pull requests, semver tags (`vX.Y.Z`) for releases; every repository's `main` is protected (PR + required checks); nothing runs in CI on a plain push.
- Container images are published to Docker Hub as `mladenovskistefan/<service>`, pinned by digest, scanned, and signed (keyless cosign) by the shared release pipeline; a Kyverno policy enforces that in the cluster.
- Free GitHub plan: the organisation shares a small number of concurrent CI jobs, so changes that touch many repositories are rolled out one repository at a time.
