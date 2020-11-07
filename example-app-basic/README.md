# Example App

## Deploy to dokku

```bash
git remote add dokku dokku@dokku.me:app-name
git push dokku
```

### Setup TLS

On the dokku server:

```bash
dokku letsencrypt app-name
```
