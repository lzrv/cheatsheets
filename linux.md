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
