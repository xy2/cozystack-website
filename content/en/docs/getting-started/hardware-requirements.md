---
title: "Hardware requirements"
linkTitle: "Hardware requirements"
description: "Plan your installation."
weight: 20
---

To estimate the hardware requirements for your Cozystack installation, first decide what you want to do with it.

The Cozystack platform runs atop of Talos Linux, which is a minimalistic operating system designed to run Kubernetes
only. It's not possible to "share" a server with some other services running.

## Proof-of-concept lab

Here are baseline requirements to run a proof-of-concept installation:

* Three physical or virtual servers of amd64/x86_64 architecture, 16 GB RAM each
* We recommend to have at least 4 CPU cores per server
* If servers are virtualized, make sure the nested virtualization is enabled, and cpu model is set to `host`, without
  emulation.
* Primary disk at least 32GB. Talos Linux will use it for the operating system, images and logs. It's better to have
  more.
* Secondary disk for replicated storage system. 100GB is a good starting point, but required size completely depends on
  what are you planning to run.
* Machines are allowed to take additional IPs (this is disallowed by default and need to be enabled in most public
  clouds), or external load balancer is available.
* Additional public IPs for ingress and virtual machines (if needed and available from your provider).

## HA applications

For production environments, it's usually required to have several replicas of your services distributed across nodes.
These services usually create significant amount of traffic between nodes. Database replicas also send traffic between
nodes. Virtual machines with live migration option require replicated volumes which create a lot of traffic. So, for
production environments, it's recommended to have several network cards of 10Gbps or more. It's possible to split
storage and other traffic to different sets of network cards.

## Distributed cluster

With some tuning, it's possible to create a [distributed cluster](/docs/operations/stretched/) with Cozystack. The
common requirements are fast and reliable network **AND** low RTT (run ping command between locations to check what is
the actual RTT). The *acceptable* RTT value is 10ms at max. Kubernetes is not designed to run in the network with big
latency. Several datacenters in a single city usually have less than 1ms latency, which is fine. And it's absolutely
unacceptable to run Kubernetes and replicated storage over the network with RTT more than 20ms.

It's also recommended to have at least 2-3 nodes per datacenter in a distributed cluster. So, if the whole datacenter
connectivity is broken, the cluster will survive the outage without much pain.

If there is no possibility to keep internal network addressing scheme (VPN between datacenters), it's possible to use
KubeSpan: a Talos Linux feature that creates WireGuard-backed full mesh VPN between nodes.

## Production clusters

For production environment, consider these options:

* Recommended but not required to have separate servers for Kubernetes control plane. It's possible to run control plane
  on the same servers as workloads, but it's easier to shut down a pure worker node for maintenance or upgrade than
  combined.
* Three worker nodes is a BARE minimum to run HA applications. If you only have three nodes, and any one of them goes
  down due to hardware failure, or for maintenance, it's an emergency. Database clusters and replicated storage will
  continue to run, bit it will be not possible to start new database instances or create new replicated volumes.
* In multi-datacenter setup, it's better to have a direct dedicated optic lines between datacenters.
* Servers must have out-of-band management capabilities (IPMI, iLO, iDRAC, etc.) to allow remote management and
  recovery.
