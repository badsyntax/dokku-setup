# Apps

## Create new apps

```bash
dokku apps:create app-name
dokku apps:report app-name
```

## Destroy apps

```bash
dokku apps:destroy app-name
```

## TLS

See [TLS](./tls.md).

## Redeploy

Redeploy from docker image:

```bash
dokku tags:deploy app-name latest
```

## Deploy app from local

In your local repo:

```bash
git remote add dokku dokku@dokku.me:app-name
```

Deploy from local:

```bash
git push dokku
```

## Deploying your default app at root

Nginx will pick the first vhost if no default host is defined, so it's a good idea to claim the root domain with an actual app.

You can either create a dedicated app for the root domain or use an existing app.

### Dedicated app

Copy [example-app-basic](./example-app-basic) and update the `app-name` string in the README to your root domain (eg `dokku.example.com`).

### Existing app

```bash
# add a domain to an app
dokku domains:add app-name dokku.example.com
```
