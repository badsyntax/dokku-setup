# Install dokku

See http://dokku.viewdocs.io/dokku/getting-started/installation/

## SSH setup

Add your local public key to the dokku server to allow password-less logins:

```ssh
cat ~/.ssh/id_rsa.pub | ssh root@dokku.me 'cat >> ~/.ssh/authorized_keys'
```

Use the same public key for setting up dokku.

### Deploying your default app at root

Refer to [apps](./apps.md).
