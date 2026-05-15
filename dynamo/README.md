# NVIDIA Dynamo on a Kubernetes Instant Cluster

Manifests for the [Deploying NVIDIA Dynamo on a Kubernetes Instant Cluster](https://docs.verda.com/clusters/instant-clusters/tutorials/dynamo-inference/) tutorial on Verda Docs.

## Files

| File | Purpose |
|---|---|
| `qwen3-quickstart.yaml` | `DynamoGraphDeploymentRequest` (DGDR) that auto-profiles the cluster and generates a `DynamoGraphDeployment` for `Qwen/Qwen3-0.6B`. |

## Fields to edit before applying

- `spec.model` — any model on the HuggingFace Hub readable by `$HF_TOKEN`.
- `spec.hardware.totalGpus` — match the allocatable GPU count reported by `kubectl get nodes -o jsonpath='{...}'`.
- `spec.hardware.numGpusPerNode` — GPUs per node (usually 8).
- `spec.hardware.vramMb` — per-GPU VRAM in MiB. `275039` is correct for B300 SXM6.
- `spec.hardware.gpuSku` — keep `b200_sxm` for both B200 and B300 (B300 is not in Dynamo's enum yet; B200 profile runs correctly on B300 SXM6 hardware).

## Usage

```bash
curl -O https://raw.githubusercontent.com/datacrunch-research/instant-cluster-examples/main/dynamo/qwen3-quickstart.yaml
# edit hardware fields for your cluster
kubectl apply -f qwen3-quickstart.yaml
```

See the [tutorial](https://docs.verda.com/clusters/instant-clusters/tutorials/dynamo-inference/) for prerequisites (NGC + HuggingFace secrets, GPU Operator install, etc.), verification, and known issues.
