# CNC notes / cheatsheet

## Installation

### Local / dev (with Kind)

* Pull images
```bash
docker pull sig-repo.synopsys.com/synopsys/cim-web:2022.6.0
docker pull sig-repo.synopsys.com/synopsys/cim-downloads:2022.6.0
docker pull sig-repo.synopsys.com/synopsys/cim-tools:2022.6.0
docker pull sig-repo.synopsys.com/synopsys/cim-database:2022.6.0
docker pull sig-repo.synopsys.com/synopsys/cov-manage-im:2022.6.0
```

* Tag images
```bash
docker tag sig-repo.synopsys.com/synopsys/cim-web:2022.6.0 localhost:5000/cim-web:2022.6.0
docker tag sig-repo.synopsys.com/synopsys/cim-downloads:2022.6.0 localhost:5000/cim-downloads:2022.6.0
docker tag sig-repo.synopsys.com/synopsys/cim-tools:2022.6.0 localhost:5000/cim-tools:2022.6.0
docker tag sig-repo.synopsys.com/synopsys/cim-database:2022.6.0 localhost:5000/cim-database:2022.6.0
docker tag sig-repo.synopsys.com/synopsys/cov-manage-im:2022.6.0 localhost:5000/cov-manage-im:2022.6.0
```

* Push images to the local repo
```bash
docker push localhost:5000/cim-web:2022.6.0
docker push localhost:5000/cim-downloads:2022.6.0
docker push localhost:5000/cim-tools:2022.6.0
docker push localhost:5000/cim-database:2022.6.0
docker push localhost:5000/cov-manage-im:2022.6.0
```

* Create kind config `kind-config.yaml`
```yaml
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
containerdConfigPatches:
  - |-
    [plugins."io.containerd.grpc.v1.cri".registry.mirrors."localhost:5000"]
      endpoint = ["http://kind-registry:5000"]
nodes:
- role: control-plane
  kubeadmConfigPatches:
  - |
    kind: InitConfiguration
    nodeRegistration:
      kubeletExtraArgs:
        node-labels: "ingress-ready=true"
        authorization-mode: "AlwaysAllow"
  extraPortMappings:
  - containerPort: 80
    hostPort: 80
    protocol: TCP
  - containerPort: 443
    hostPort: 443
    protocol: TCP
```

* Create kind cluster
```bash
kind create cluster --config=kind-config.yaml
```

* Clone CNC repo

* Customize certs

  1. create a new cert including the current hostname
  NOTE: go cannot parse encrypted keys, so create an unecnrypted one(w/o pswd)

```bash
GENERATE NEW CERT/KEY/CACERT

1/ Generate private key:
openssl genrsa -des3 -out ca.key 4096

1.5/ (optional) Verify contents of the key:
openssl rsa -noout -text -in ca.key

2/ Generate CA cert:
openssl req -new -x509 -days 365 -key ca.key -out ca.cert.pem

2.5/ (optional) Verify CA cert contents:
openssl x509 -noout -text -in ca.cert.pem

3/ Generate server key:
openssl genrsa -des3 -out server.key 4096

4/ Generate CSR:
Important: put a hostname(FQDN) or server IP in Common Name field of the CSR
openssl req -new -key server.key -out server.csr

5/ Sign CSR with CA cert and issue server certificate:
openssl x509 -req -days 365 -in server.csr -CA ca.cert.pem -CAkey ca.key -CAcreateserial -out server.crt

Additional:
6/ Create self-signed server certificate in one command:
openssl req  -nodes -new -x509  -keyout ssl.key -out ssl.cert
```

  2. Move the custom key and cert to `cnc-umbrella-chart/local-dev/certs`

* configure the helm chart to use your server host name instead of local.cim.com
  - change the following lines to the actual machine hostname in the `local-dev/cnc/values.yaml`
  ```bash
  local-dev/cnc/values.yaml:      - local.cim.com
  local-dev/cnc/values.yaml:          - local.cim.com
  local-dev/cnc/values.yaml:      url: "https://local.cim.com"
  ```

* Run install script
```bash
cd local-dev
./deploy-cnc.sh
```

* Uninstall / cleanup
```bash
kind delete cluster
```
