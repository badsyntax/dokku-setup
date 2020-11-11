# Maintenance

As you create docker images outside of the dokku api you can quickly loose track of the created images, networks and containers.

View disk usage:

```bash
docker system df
```

Use the dokku cleanup tool:

```bash
dokku cleanup
```

Use docker prune:

```bash
docker system prune
```
