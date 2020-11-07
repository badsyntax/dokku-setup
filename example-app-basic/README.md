# Example App

## dokku setup

```bash
git remote add dokku dokku@dokku.me:app-name
```

Push to deploy:

```bash
git push dokku
```

On the dokku server:

```bash
dokku letsencrypt app-name
```
