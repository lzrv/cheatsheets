# Useful k8s commands/info

## Kind

### Installation / Quickstart (Linux)

* Download the latest binary [here](https://github.com/kubernetes-sigs/kind/releases).

```bash
wget https://github.com/kubernetes-sigs/kind/releases/download/v0.14.0/kind-linux-arm64

# validate checksum
wget https://github.com/kubernetes-sigs/kind/releases/download/v0.14.0/kind-linux-amd64.sha256sum
sha256sum -c kind-linux-amd64.sha256sum

# move
mv kind-linux-amd64 ~/bin/kind
chmod u+x ~/bin/kind
```

* create/delete cluster
```bash
kind [create|delete] cluster
```

## kubectl

* Change namespace context
```bash
kubectl config set-context --current --namespace [ns]
```

## Helm

* List the installed charts
```bash
```
