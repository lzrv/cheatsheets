# BD cheatsheet

## K8s install

### Helm

* (Guide)[https://github.com/blackducksoftware/hub/tree/master/kubernetes/blackduck]
* Modify values.yaml to use internal postgres(set to use external by default
```
postgres:
  isExternal: true -> isExternal: false
```

* Install
```bash
  helm install ${BD_NAME} synopsys/blackduck -n ${BD_NAME} -f values.yaml -f ${BD_SIZE}.yaml
```

#### Issues:
* Helm install fails with(Helm 3):
```bash
$ helm install ${BD_NAME} synopsys/blackduck --namespace ${BD_NAME} -f ${BD_SIZE}.yaml --set tlsCertSecretName=${BD_NAME}-blackduck-webserver-certificate
Error: INSTALLATION FAILED: failed pre-install: unable to build kubernetes object for pre-install hook blackduck/templates/postgres-config.yaml: error validating "": error validating data: unknown object type "nil" in ConfigMap.data.HUB_POSTGRES_HOST
```

## Detect examples

### Rapid scan

bash <(curl -s -L https://detect.synopsys.com/detect7.sh) \
--blackduck.url=$URL \
--blackduck.api.token=$TOKEN \
--blackduck.trust.cert=true \
--detect.source.path=$TARGET \
--detect.blackduck.scan.mode=RAPID \
--detect.bom.aggregate.name=aggregated.bdio \
--detect.cleanup=false
