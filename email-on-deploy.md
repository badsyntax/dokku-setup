# email deploy notifications

It can be useful to receive email notifications on successful dokku deploys:

```bash
dokku plugin:install https://github.com/badsyntax/dokku-email.git
dokku email:add email@example.com
```

This requires you have setup email on your host dokku server. See [email](email.md).
