# install dokku

Add your public key to your server to allow password-less logins:

```ssh
cat ~/.ssh/id_rsa.pub | ssh root@dokku.me 'cat >> ~/.ssh/authorized_keys'
```

Use the same public key for setting up dokku.
