# Create new apps

On your dokku server:

```bash
dokku apps:create app-name
dokku apps:report app-name
```

Add TLS:

```bash
dokku letsencrypt app-name
```

Redeploy from docker image:

```bash
dokku tags:deploy app-name latest
```

Now in your local repo:

```bash
git remote add dokku dokku@dokku.me:app-name
```

Deploy from local:

```bash
git push dokku
```
