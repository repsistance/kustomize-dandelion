# Requirements

* Kubernetes. For local k8s development environment, we encourage you to use [k3s]/[k3d] lightweight k8s distro.
* kubectl
* [Kustomize]

# Render k8s manifests

``` 
OVERLAY=mainnet
kustomize build --enable_alpha_plugins overlays/${OVERLAY} > overlays/${OVERLAY}/output.yaml
```

# Deploy k8s manifests

```
NAMESPACE=dandelion-mainnet
kubectl get ns ${NAMESPACE} || kubectl create ns ${NAMESPACE}
kubectl apply -n ${NAMESPACE} -f overlays/${OVERLAY}/output.yaml
```

# Operations

* Check sync progress
```
curl -kL 'https://graphql-api.mainnet.local/' \
     -H 'Accept-Encoding: gzip, deflate, br' \
     -H 'Content-Type: application/json' \
     -H 'Accept: application/json' \
      --data-binary '{"query":"query cardanoDbSyncProgress {\n    cardanoDbMeta {\n        initialized\n        syncPercentage\n    }\n}\n"}'
```

[kustomize]: https://kustomize.io/
[k3d]: https://github.com/rancher/k3d
[k3s]: https://k3s.io/
