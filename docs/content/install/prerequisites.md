# MaaS Installation Overview

_Models-as-a-Service_ is compatible with the Open Data Hub project (ODH) and
Red Hat OpenShift AI (RHOAI). MaaS is installed by enabling it in the DataScienceCluster resource:

* [Install your platform](platform-setup.md) (ODH or RHOAI operators and DSCInitialization).
* [Install MaaS Components](maas-setup.md) (Database, Gateways, DataScienceCluster).

## Version Compatibility

| MaaS Version | OCP | Kuadrant (ODH) / RHCL (RHOAI) | Gateway API |
|--------------|-----|-------------------------------|-------------|
| v0.0.2       | 4.19.9+ | v1.3+ / v1.2+             | v1.2+       |
| v0.1.0+      | 4.19.9+ | v1.4.2+ / v1.3            | v1.2+       |

!!! note "Other Kubernetes flavors"
    Other Kubernetes flavors (e.g., upstream Kubernetes, other distributions) are currently being validated.



## Required Tools

The following tools are used across the installation guides:

* `kubectl` or `oc` — cluster access
* `curl` — used by Operator Setup (ODH/LWS)
* `jq` — used for validation and version parsing
* `kustomize` — used for Gateway AuthPolicy (MaaS Components)
* `envsubst` — used for policy templates (MaaS Components)

## Requirements for Open Data Hub project

MaaS requires Open Data Hub version 3.0 or later, with the Model Serving component
enabled (KServe) and properly configured for deploying models with `LLMInferenceService`
resources.

## Requirements for Red Hat OpenShift AI

MaaS requires Red Hat OpenShift AI (RHOAI) version 3.0 or later, with the Model Serving
component enabled (KServe) and properly configured for deploying models with
`LLMInferenceService` resources.

A specific requirement for MaaS v0.2.0+ is to set up RHOAI Model Serving with Red Hat Connectivity Link (RHCL) v1.3 or later.

## Authorino TLS Configuration (Required)

Authorino must have TLS enabled on its gRPC listener (`listener.tls.enabled: true`) for production deployments. This secures the Gateway → Authorino authentication traffic and protects API credentials in transit.

!!! warning "Do not skip TLS in production"
    Authorino can operate without TLS, and some examples show `tls.enabled: false` for local development. However, disabling TLS in production exposes API key credentials on the wire between the Gateway and Authorino. Always enable TLS for any non-development deployment.

TLS is configured automatically when using `./scripts/deploy.sh` or `./scripts/setup-authorino-tls.sh`. For manual setup or custom deployments, see [TLS Configuration — Authorino TLS Configuration](../configuration-and-management/tls-configuration.md#authorino-tls-configuration).

## Optional: Observability Prerequisites

If you plan to use MaaS dashboards, showback, or usage metrics, additional platform configuration is required:

- **User Workload Monitoring** — Required for Prometheus to scrape metrics from MaaS components
- **Kuadrant Observability** — Required for rate-limiting and usage metrics (e.g., `authorized_calls`, `limited_calls`)

See [Observability Prerequisites](../advanced-administration/observability.md#prerequisites) for detailed configuration steps.
