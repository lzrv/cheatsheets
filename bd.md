# BD cheatsheet

## K8s install

### Helm

* [Guide](https://github.com/blackducksoftware/hub/tree/master/kubernetes/blackduck)

* Create BD namespace:
```bash
kubectl create ns ${BD_NAME}
```

*
* Modify values.yaml to use internal postgres(set to use external by default
```
postgres:
  isExternal: true -> isExternal: false
```

* Install
```bash
  helm install ${BD_NAME} synopsys/blackduck -f values.yaml
```

#### Issues:
* Helm install fails with(Helm 3):
```bash
$ helm install ${BD_NAME} synopsys/blackduck --namespace ${BD_NAME} -f ${BD_SIZE}.yaml --set tlsCertSecretName=${BD_NAME}-blackduck-webserver-certificate
Error: INSTALLATION FAILED: failed pre-install: unable to build kubernetes object for pre-install hook blackduck/templates/postgres-config.yaml: error validating "": error validating data: unknown object type "nil" in ConfigMap.data.HUB_POSTGRES_HOST
```
* Type LoadBalancer is not supported by Kind out of the box.

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

## Troubleshooting

### check webserver access log
```bash
grep -Po '"GET /api/\w+' hub-webserver/access-log/2022-09-22.log | sort | uniq -c | sort -rn | head
```

## LDAP integration

```
Server URL: ldap://<ldapserver>:389 for ldap and 636 for ldaps

Authentication Type: Typically Simple

Manager DN: Must be a user that can authenticate and bind users. Also need full DN path of the Service Account (CN=service_account,OU=Administrators,OU=People,DC=company,DC=com)

Manager Password: Password for the Manager DN

User Search Base: Where you are searching for users, examples (DC=company, DC=com will search all users in that DC, DC) (OU=People, OU=Sales, DC=company, DC=com will only search for users that are people in Sales OU)

User Search Filter: For AD sAMAccountName={0} for OpenLDAP uid={0} (these may be different but these are the defaults)

User DN Pattern: Not needed

First Name: Depends on attribute configuration but typically givenName

Last Name: Depends on attribute configuration but typically sn

Email: Depends on attribute but configuration typically mail

Group Search Base: Same as user search base but for the groups you want

Group Filter: Typically one of the following member={0}, members={0}, memberOf={0}

(&(sAMAccountName={0})(memberOf=CN=bdhub,OU=Groups,DC=blackduck,DC=com))

This would allow only users that are a memberOf the bdhub group to log into the Hub.
```
