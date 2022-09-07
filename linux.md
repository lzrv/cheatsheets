# Useful bash commands for Linux administration

## Misc

* Get number of CPU cores
```bash
grep -c ^processor /proc/cpuinfo
```

## SSH

* generate key with your email
```bash
ssh-keygen -C email@example.com
```

* Validate ssh connection
```bash
ssh -T git@sig-gitlab

#verbose
ssh -Tv git@sig-gitlab
```

## SSL

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
