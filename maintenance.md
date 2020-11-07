# Maintenance

As you create docker images outside of the dokku api you can quickly loose track of the created images, networks and containers.

View disk usage:

```bash
docker system df
```

Use docker prune to free up resources:

```bash
docker system prune
```
