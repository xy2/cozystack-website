---
title: "Deploy your application"
linkTitle: "Deploy your application"
description: "Learn how to deploy an example application in Cozystack tenant"
weight: 40
---

In this guide we will deploy an application (that is not a part of Cozystack) in the tenant.

We will reach the following goals:

* Create a tenant in Cozystack to make an isolated space for a development team
* Retrieve the tenant kubernetes credentials
* Create a managed database in the root tenant (for maximum performance)
* Create a cache instance in the development tenant (to show that we can use the Cozystack application in different
  tenants)
* Deploy a sample application as a usual Kubernetes deployment

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

## Create a database

The Cozystack framework includes a managed database system. It allows you to create a database in the root tenant for
maximum performance, or in child tenants for maximum isolation. In this example, we create a database in the root
tenant.

TODO add app postgresql / instagram

TODO show how secrets copied

## Create a cache service

TODO use a token from the tenant kubeconfig to access dashboard

TODO show that database credentials are in place

TODO add app redis / instagram

## Deploy an application with helm

TODO show helm chart values excerpt

TODO show helm upgrade --install with tenant kubeconfig

TODO show kubectl get pods and ingress (port 80! no https yet)
