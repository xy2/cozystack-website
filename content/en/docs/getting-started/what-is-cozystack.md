---
title: "About Cozystack framework"
linkTitle: "Deploy and Configure Cozystack"
description: "Learn how to get a running Cozystack cluster on bare metal or VMs in a series of guided steps."
weight: 10
---

Cozystack is a Kubernetes-based framework for building a complete cloud environment on your (or rented) hardware.
Numerous components that are usually installed in Kubernetes clusters are already included in the Cozystack framework
and tuned to work seamless with each other. The virtualization platform is also included and does not require extra
hardware servers, you get your virtual machines right in the Kubernetes. And another powerful feature is
the [tenant](/docs/guides/concepts/#tenant-system) system that allows you to isolate teams, individual developers or
even companies in their own fully-functional space, all on the same hardware.

Key features
------------

### Multi-user, multi-tenant, SSO included

Cozystack is designed to be used by multiple teams, departments or even companies. The "usual" approach of giving out
the dedicated namespace to each team could be not convenient. Users may require to have multiple environments with same
fixed namespace name. And user can suffer from not having root permission on the cluster. The Cozystack tenant system
allows to create Kubernetes-in-Kubernetes with a single app deployment. The users of downward Kubernetes can have full
access to it. Quota system allows to get the maximum from the hardware, while keeping the resources isolated and
preventing the "noisy neighbor" problem.

It's possible for Cloud providers to get a detailed usage report for each tenant.

The SSO subsystem is based on Keycloak and allows to create a single sign-on system. The Kubernetes API in the root
tenant as well as in downward tenants supports SSO out of the box. So, instead of rotating tokens when you need to lock
access, it's possible to just block a user account in Keycloak.

### Storage system included

Not all businesses could afford a dedicated hardware SAN or NAS. Cozystack includes a reliable distributed storage
system that allows to create disaster-prone replicated volumes. It's even possible to replicate volumes across multiple
data centers.

### Virtualization system included

Usually, you either have a Virtualization or a Containerization system. Cozystack combines both of them in one. You
don't have to build another system on another set of servers to run virtual machines. Virtual machines consume
resources (CPU, memory, GPU, storage) right from Kubernetes capacity.

### Managed databases do not have virtualization overhead

While Linux-on-Linux virtualization is extremely efficient, it still can have some overhead. In Cozystack, managed
databases are running in the containers, but right on the hardware. You may create multiple HA databases, each with own
address on a limited hardware, but all databases are running with direct access to CPU and storage.

### Kubernetes ecosystem

We do not know another Kubernetes distribution that has more infrastructure components included (seriously, send us
a link if you find one!). Instead of installing all the components and controllers, you just choose
a [bundle](/docs/operations/bundles/) that fits your needs. All the components are pre-configured and tested to work
with each other. Components get updates along with Cozystack framework.
