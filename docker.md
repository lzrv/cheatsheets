# Docker cheatsheet

## Performance

### Basics
```bash
docker stats
```

### Detecting containers with high I/O
```bash
docker stats --format "table {{.Container}}\t{{.BlockIO}}" --no-stream --no-trunc
```

### More perf info about a container
```bash
docker stats $CONTAINER_ID --format "table {{.Container}}\t{{.BlockIO}}\t{{.CPUPerc}}\t{{.MemPerc}}\t{{.MemUsage}}" --no-stream
```

### Restarting a Docker Swarm service
```bash
docker service update --force $serice_id
```
Run `docker service ls` or `docker stack services $stack_name` to get the $service_id
