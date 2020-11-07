# TLS with letsencrypt

Adding TLS to your apps is straightforward:

```bash
dokku plugin:install https://github.com/dokku/dokku-letsencrypt.git
dokku config:set --global DOKKU_LETSENCRYPT_EMAIL=email@gmail.com
dokku letsencrypt:cron-job --add
```

Setup TLS for an app:

```bash
dokku letsencrypt myapp
```
