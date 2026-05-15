# instant-cluster-examples

Runnable YAML manifests and configs for [Verda Instant Cluster](https://docs.verda.com/clusters/instant-clusters/) tutorials.

Each subdirectory pairs with a tutorial on [docs.verda.com](https://docs.verda.com). The tutorials cover *why* and *how*; this repo holds the long-form YAMLs so they stay readable and easy to copy without cluttering the prose.

## Tutorials

| Directory | Tutorial |
|---|---|
| [`kueue-skypilot/`](kueue-skypilot/) | [Gang-scheduled Multi-node Training with SkyPilot + Kueue](https://docs.verda.com/clusters/instant-clusters/tutorials/kueue-gang-scheduling/) — multi-node TorchTitan run on B300, gang-scheduled via Kueue. |
| [`dynamo/`](dynamo/) | [Deploying NVIDIA Dynamo on a Kubernetes Instant Cluster](https://docs.verda.com/clusters/instant-clusters/tutorials/dynamo-inference/) — disaggregated LLM inference with auto-profiling. |

## Usage pattern

The tutorials reference these files by raw URL:

```bash
curl -O https://raw.githubusercontent.com/datacrunch-research/instant-cluster-examples/main/<dir>/<file>.yaml
# edit the fields called out in the tutorial / subdirectory README
kubectl apply -f <file>.yaml          # or: sky launch -y <file>.yaml
```

Each subdirectory has its own `README.md` listing fields to edit before applying.

## Contributing

PRs welcome — especially additional examples (other model backends, smaller GPU SKUs, alternative schedulers). Keep examples minimal and runnable; prose belongs in the docs.

## License

MIT — see [LICENSE](LICENSE).
