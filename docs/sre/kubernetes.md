---
title: Kubernetes
layout: default
parent: SRE
nav_order: 40
---

# Kubernetes

## Debugging map

- Pods: restarts, readiness, logs, events.
- Deployments: rollout state, replica availability, image versions.
- Services: endpoints, selectors, DNS, network policies.
- Nodes: pressure, allocatable resources, kubelet health.
- Control plane: API server, scheduler, controller manager, etcd.

## Useful commands to document

```bash
kubectl get pods -A
kubectl describe pod <pod> -n <namespace>
kubectl logs <pod> -n <namespace> --previous
kubectl get events -A --sort-by=.lastTimestamp
```

{: .note }
Replace placeholders with environment-specific commands once the target clusters are known.
