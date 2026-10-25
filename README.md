# Kubernetes Platform Lab

A small, reviewable Kubernetes deployment example built around an unprivileged
NGINX container. The manifests demonstrate workload security, probes, resource
budgets, a ClusterIP service, and optional autoscaling.

## Requirements

- A reachable Kubernetes cluster and a configured `kubectl` context
- `kubectl` with Kustomize support
- Metrics Server installed if applying the HorizontalPodAutoscaler

## Deploy

Review the manifests and confirm the active context before applying:

```powershell
kubectl config current-context
kubectl diff -k .
kubectl apply -k .
kubectl get all -n platform-lab
```

The app is exposed as a `ClusterIP`. For a local cluster, use port-forwarding:

```powershell
kubectl port-forward -n platform-lab service/web 8080:80
```

Then visit `http://localhost:8080`.

Remove the lab resources:

```powershell
kubectl delete -k .
```

## What to explore

- A dedicated namespace and labels for ownership.
- A non-root container with all Linux capabilities dropped.
- A read-only root filesystem and a temporary writable mount.
- CPU/memory requests and limits.
- Startup, readiness, and liveness probes.
- A `ClusterIP` service rather than an internet-facing load balancer.
- An HPA that requires cluster metrics and has bounded replicas.

## Production follow-ups

- Replace the sample image with an image built by the Docker lab, pinned by
  digest, and published to a registry accessible by the cluster.
- Add an ingress controller, TLS, network policies, and workload identity as
  required by the platform.
- Review the image and manifest against your organization's policy.
- Set resource budgets and autoscaling based on observed workload behavior.
- Configure logs, metrics, and alerts before relying on the service.

## Cost and cleanup

These manifests create Kubernetes objects only. Cluster, node, public IP,
registry, and monitoring charges depend on your cloud provider and cluster.
The service is internal; no external load balancer is provisioned by this lab.
