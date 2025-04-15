---
title: "Deploy your application"
linkTitle: "Deploy your application"
description: "Learn how to deploy an example application in Cozystack tenant"
weight: 50
---

In this guide we will deploy an application (that is not a part of Cozystack) in the tenant.

We will reach the following goals:

* Create a managed database in the root tenant (for maximum performance)
* Create a cache instance in the development tenant (to show that users can freely deploy Cozystack applications in
  their tenant)
* Deploy a sample application as a usual Kubernetes deployment
* Show how to access the tenant as an end user

The imaginary application we will deploy is named `instaphoto`. And development team that will use it is named `team1`.
Tenant is named after the team.

## Prerequisites

We will need a running Cozystack installation and tenant credentials. You can use
the [installation guide]({{% ref "/docs/getting-started/first-deployment" %}}) to install it. The target tenant will be
named `team1` in this guide.

The DNS name is required to test the application. In this example, we will use a subdomain of main Cozystack domain.

Apps can be installed using plain `kubectl apply` or `helm upgrade`, with GitOps approach, or using the dashboard. If
you choose the dashboard method, ensure dashboard ingress is accessible as described in
the [installation guide]({{% ref "/docs/getting-started/first-deployment#cozystack-dashboard" %}}).

Dashboard installation method is the simplest to begin with. Installed apps could be later converted to the IaC
approach.

## Create a database

The Cozystack framework includes a managed database system. It allows you to create a database in the root tenant for
maximum performance, or in child tenants for maximum isolation. In this example, we create a database in the root
tenant. But the database will be accessible in other tenants.

{{< tabs name="create_database" >}}
{{% tab name="in Dashboard" %}}

1. Open the dashboard as a tenant-root user.
2. Ensure current context is `tenant-root`. Switch context and reaload the page if needed.
3. Click on the `Catalog` tab in the left menu.
4. Search for `Postgres` application badge and click on it. Application builtin documentation will open.
5. Read the documentation and click on the `Deploy` button. Application parameters page will open.
6. Parameters are pre-filled with sane and minimal defaults. You can change them in a Visual editor or in YAML editor.
   YAML editor contains full commentaries. You can also switch between editors back and forth. Do not worry if unsure
   for right parameters. It can be changed later. The only mandatory parameter is `name`. It should be unique in this
   tenant.
7. As we are going to publish this database across tenants, ensure `external` flag is set to true.
8. When finished with parameters, click the `Deploy` button again. The application will be installed in the root tenant.

{{% /tab %}}

{{% tab name="with kubectl" %}}

Create a manifest `postgres.yaml` with the following content:

```yaml
apiVersion: helm.toolkit.fluxcd.io/v2
kind: HelmRelease
metadata:
  name: postgres-instaphoto-prod
  namespace: tenant-root
spec:
  chart:
    spec:
      chart: postgres
      reconcileStrategy: Revision
      sourceRef:
        kind: HelmRepository
        name: cozystack-apps
        namespace: cozy-public
      version: 0.10.0
  interval: 0s
  values:
    databases:
      myapp:
        roles:
          admin:
            - user1
    external: true
    replicas: 2
    resourcesPreset: nano
    size: 5Gi
    users:
      user1:
        password: strongpassword
```

and apply it:

```bash
kubectl apply -f postgres.yaml
```

The contents of manifest can be copied from the resource that was created in the dashboard and then edited.

{{% /tab %}}
{{< /tabs >}}

TODO show how secrets copied

## Create a cache service

From this point we will use the tenant credentials to access the platform. Use the tenant's kubeconfig for kubectl and
token from it to access the dashboard.

{{< tabs name="create_redis" >}}
{{% tab name="in Dashboard" %}}

1. Open the dashboard as a tenant-team1 user.
2. Ensure current context is `tenant-team1`. Logout and login again if not.
3. Follow the same steps as with postgresql, but choose redis and leave `external` to false (default). Otherwise,
   sibling tenants could access your redis over the network.
4. The redis application has `authEnabled` parameter that will create us a default user. That's enough for our
   application.
5. When finished with parameters, click the `Deploy` button. The application will be installed in the `team1` tenant.

{{% /tab %}}

{{% tab name="with kubectl" %}}

Create a manifest `redis.yaml` with the following content:

```yaml
apiVersion: helm.toolkit.fluxcd.io/v2
kind: HelmRelease
metadata:
  name: redis-instaphoto
  namespace: tenant-team1
spec:
  chart:
    spec:
      chart: redis
      reconcileStrategy: Revision
      sourceRef:
        kind: HelmRepository
        name: cozystack-apps
        namespace: cozy-public
      version: 0.6.0
  interval: 0s
  values:
    authEnabled: true
    external: false
    replicas: 2
    resources: {}
    resourcesPreset: nano
    size: 1Gi
```

and apply it:

```bash
kubectl apply -f redis.yaml
```

{{% /tab %}}
{{< /tabs >}}

After a while, the redis application will be installed in the `team1` tenant. The generated password could be found in
the dashboard.

{{< tabs name="redis_password" >}}
{{% tab name="in Dashboard" %}}

1. Open the dashboard as a tenant-team1 user.
2. Click on the `Applications` tab in the left menu.
3. Find the `redis-instaphoto` application and click on it.
4. The password is shown in the `Secrets` section. There are the show/copy buttons next to it.

{{% /tab %}}

{{% tab name="with kubectl" %}}

```bash
# Use the tenant kubeconfig
export KUBECONFIG=./kubeconfig-tenant-team1
# Get the password
kubectl -n tenant-team1 get secret redis-instaphoto-auth
```

{{% /tab %}}
{{< /tabs >}}

## Deploy an application with helm

The rest of journey is the same as with any other Kubernetes cluster. You can use `kubectl` or `helm` or your CI/CD
system to deploy kubernetes-native applications. The passwords for the database and redis are stored in the kubernetes
secrets. You can copy-paste them once to your application, or create a reference to existing secret to get them
automatically on deployment.
