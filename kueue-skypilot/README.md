# Gang-scheduled multi-node training: SkyPilot + Kueue

Manifests for the [Gang-scheduled Multi-node Training with SkyPilot + Kueue](https://docs.verda.com/clusters/instant-clusters/tutorials/kueue-gang-scheduling/) tutorial on Verda Docs.

## Files

| File | Purpose |
|---|---|
| `kueue-queue.yaml` | `ResourceFlavor` + `ClusterQueue` + `LocalQueue` definitions. Sized for a 2 × B300 cluster (16 GPUs, 480 vCPU, 4000 Gi RAM, 2 RDMA devices). |
| `torchtitan-sky.yaml` | SkyPilot task spec. Launches a 2-node TorchTitan training job on 16 GPUs (2 × 8 B300), gated by Kueue via `local_queue_name: my-local-queue`. |

## Fields to edit before applying

### `kueue-queue.yaml`

- `spec.resourceGroups[0].flavors[0].resources[*].nominalQuota` — set each value to your cluster's actual capacity (`N_nodes × per_node`). Mismatch causes Kueue to over- or under-admit.
- `spec.resourceGroups[0].coveredResources` — drop `rdma/rdma_shared_device_a` if your cluster does not expose it.
- `metadata.namespace` on the `LocalQueue` — must match a Kueue-managed namespace (default: `default`).

### `torchtitan-sky.yaml`

- `num_nodes` — number of pods to launch.
- `resources.accelerators` — e.g. `B300:8`, `H100:8`, `A100-80GB:8`.
- `config.kubernetes.kueue.local_queue_name` — must match a `LocalQueue` in the kubeconfig's current-context namespace.

## Usage

```bash
# Queue/quota
curl -O https://raw.githubusercontent.com/datacrunch-research/instant-cluster-examples/main/kueue-skypilot/kueue-queue.yaml
# edit nominalQuota and any coveredResources you need
kubectl apply -f kueue-queue.yaml

# Training job
curl -O https://raw.githubusercontent.com/datacrunch-research/instant-cluster-examples/main/kueue-skypilot/torchtitan-sky.yaml
# edit num_nodes / accelerators / local_queue_name
sky launch -c tt-tutorial -y torchtitan-sky.yaml
```

See the [tutorial](https://docs.verda.com/clusters/instant-clusters/tutorials/kueue-gang-scheduling/) for the full walk-through, prerequisites, verification, and troubleshooting.
