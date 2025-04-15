---
title: "Create tenant and get access to it"
linkTitle: "Create tenant"
description: "Learn how to create a tenant and get credentials to access it"
weight: 40
---

## Prerequisites

We will need a running Cozystack installation. You can use
the [installation guide]({{% ref "/docs/getting-started/first-deployment" %}}) to install it.

Let's assume for this tutorial that OIDC is disabled.

While installing Talos for Cozystack you should have get the `KUBECONFIG` for you new cluster. This file is required to
bootstrap the framework. It's also possible to use this `KUBECONFIG` to create a new tenant and a database in the root
tenant.

{{% alert color="warning" %}}
:warning: This `KUBECONFIG` should not be used for day-to-day operations. It's much safer to use OIDC, but we skip it in
this tutorial to keep it short.
{{% /alert %}}

While GitOps is not configured yet, it's possible to use the dashboard to install apps.

Ensure ingress is working and dashboard is accessible as described in
the [installation guide]({{% ref "/docs/getting-started/first-deployment#cozystack-dashboard" %}}).

## Create a tenant

TODO add app tenant / team1

TODO get/make kubeconfig for the tenant

### Get tenant kubeconfig with OIDC enabled


### Get tenant kubeconfig with OIDC disabled
