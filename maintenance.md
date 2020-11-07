# Maintenance

As you create docker images outside of the dokku api you can quickly loose track of the created images, networks and containers.

Use docker prune to prune everything that is not required or dangling.

```bash
docker system prune
```
