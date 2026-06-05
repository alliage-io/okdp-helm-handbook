[![Helm](https://img.shields.io/badge/helm-3+-blue.svg)](https://helm.sh/)
[![Kubernetes](https://img.shields.io/badge/kubernetes-1.19+-blue.svg)](https://kubernetes.io/)
[![License Apache2](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](http://www.apache.org/licenses/LICENSE-2.0)

# Prerequisites umbrella chart

A Helm chart for installing OKDP prerequisites.

## Installing the chart

To install the chart with the release name `prerequisites`:

```sh
# 1. cert-manager + issuers
helm dependency update modules/cert-manager
helm install cert-manager modules/cert-manager \
  -f modules/cert-manager/values/sandbox.yaml \
  -n okdp-prerequisites \
  --create-namespace \
  --wait

# 2. trust-manager
helm dependency update modules/trust-manager
helm install trust-manager modules/trust-manager \
  -f modules/trust-manager/values/sandbox.yaml \
  -n okdp-prerequisites \
  --create-namespace \
  --wait

# 3. prerequisites (no cert-manager dependency, deploys independently)
helm dependency update modules/prerequisites/
helm install prerequisites modules/prerequisites \
  -f modules/prerequisites/values/sandbox.yaml \
  -n okdp-prerequisites \
  --create-namespace
```

## Teardown

```sh
# uninstalling the chart prerequisites
helm uninstall prerequisites -n okdp-prerequisites
# Uninstalling the chart trust-manager
helm uninstall trust-manager -n okdp-trust-manager
# uninstalling the chart cert-manager
helm uninstall cert-manager -n okdp-cert-manager
kubectl delete crd bundles.trust.cert-manager.io
kubectl delete namespace okdp-prerequisites
```
