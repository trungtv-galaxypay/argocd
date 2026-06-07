# GitOps Multi-Environment with ArgoCD ApplicationSet

## Overview

This repository manages multiple microservices using Helm Charts and deploys them automatically through ArgoCD ApplicationSet.

Each service can be deployed to multiple environments (e.g., dev, prd, staging).

When a new service or environment is added, ArgoCD automatically creates the corresponding Application without requiring any changes to the ApplicationSet configuration.

---

# Directory Structure

```text
services/
├── payment-service/
│   ├── Chart.yaml
│   ├── templates/
│   │   ├── deployment.yaml
│   │   └── service.yaml
│   └── envs/
│       ├── values-dev.yaml
│       └── values-prd.yaml
│
└── order-service/
    ├── Chart.yaml
    ├── templates/
    │   ├── deployment.yaml
    │   └── service.yaml
    └── envs/
        ├── values-dev.yaml
        └── values-prd.yaml
│
applicationsets/
└── services-appset.yaml
```

---

# Directory Description

## services/

Contains all Helm Charts for the microservices.

Example:

```text
services/order-service
```

represents a single application.

---

## envs/

Contains environment-specific configuration files.

Example:

### values-dev.yaml

```yaml
replicaCount: 1

image:
  repository: order-service
  tag: dev

env: dev
namespace: dev
```

### values-prd.yaml

```yaml
replicaCount: 3

image:
  repository: order-service
  tag: v1.0.0

env: prd
namespace: prd
```

---

## env

Defines the deployment environment.

Example:

```yaml
env: dev
```

This value is used by the ApplicationSet to generate the Application name.

---

## namespace

Defines the Kubernetes namespace where the application will be deployed.

Example:

```yaml
namespace: dev
```

ArgoCD will deploy the application into:

```text
Namespace: dev
```

---

# ApplicationSet

The ApplicationSet scans the following path:

```text
services/*/envs/*.yaml
```

Example:

```text
services/order-service/envs/values-dev.yaml
services/order-service/envs/values-prd.yaml
services/payment-service/envs/values-dev.yaml
services/payment-service/envs/values-prd.yaml
```

Each file generates one ArgoCD Application.

---

# Application Generation Example

File:

```text
services/order-service/envs/values-dev.yaml
```

Generates:

```text
order-service-dev
```

File:

```text
services/order-service/envs/values-prd.yaml
```

Generates:

```text
order-service-prd
```

---

# Generated Applications

ArgoCD automatically creates:

```text
order-service-dev
order-service-prd
payment-service-dev
payment-service-prd
```

---

# Adding a New Service

Create a new directory:

```text
services/invoice-service
```

Example:

```text
services/
└── invoice-service/
    ├── Chart.yaml
    ├── templates/
    └── envs/
        ├── values-dev.yaml
        └── values-prd.yaml
```

Commit and push the changes to Git.

ArgoCD will automatically create:

```text
invoice-service-dev
invoice-service-prd
```

No changes to the ApplicationSet are required.

---

# Adding a New Environment

For example, add:

```text
services/order-service/envs/values-staging.yaml
```

```yaml
replicaCount: 2

image:
  repository: order-service
  tag: staging

env: staging
namespace: staging
```

ArgoCD will automatically create:

```text
order-service-staging
```

No changes to the ApplicationSet are required.
