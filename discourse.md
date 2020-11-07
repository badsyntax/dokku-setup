# Setting up discourse

See https://gist.github.com/julienma/a101a72fdd97932bf28909633f45c7be

(Note, it seems we can only run one discourse container per dokku server as internall it uses a hardcoded path `/var/discourse` and thus we cannot create different volumes for different containers.)

```bash
git clone https://github.com/discourse/discourse_docker.git /var/discourse
cd /var/discourse
cp samples/standalone.yml containers/app.yml
vi containers/app.yml
```

Now update the following vars:

```yml
DISCOURSE_HOSTNAME
DISCOURSE_DEVELOPER_EMAILS
DISCOURSE_SMTP_ADDRESS
DISCOURSE_SMTP_PORT
DISCOURSE_SMTP_USER_NAME
DISCOURSE_SMTP_PASSWORD
```

Create the docker image:

```bash
./launcher bootstrap app
```

Create the app:

```bash
dokku apps:create app-name
dokku domains:set app-name app-name.com
dokku proxy:ports-add app-name http:80:80
dokku proxy:ports-remove app-name http:80:5000
```

Get run environment vars:

```bash
./launcher start-cmd app
```

Now copy all the env vars (-e) and set them as docker options, for example:

```bash
dokku config:set --no-restart app-name LANG=en_US.UTF-8 RAILS_ENV=production UNICORN_WORKERS=3 UNICORN_SIDEKIQS=1 etc....
```

Mount storage volumes:

```bash
dokku storage:mount app-name /var/discourse/shared/standalone:/shared
dokku storage:mount app-name /var/discourse/shared/standalone/log/var-log:/var/log
```

Add docker options:

```bash
dokku docker-options:add app-name run,deploy "--entrypoint /sbin/boot"
dokku docker-options:add app-name run,deploy "--hostname dokku-discourse"
dokku docker-options:add app-name run,deploy "--shm-size=512m"
dokku docker-options:add app-name run,deploy "--mac-address 00:00:00:00:00:00"
dokku ps:set-restart-policy app-name always
```

Deploy

```bash
docker tag local_discourse/app:latest dokku/app-name:latest
dokku tags:deploy app-name latest
```

Setup TLS:

```bash
dokku letsencrypt app-name
```

## Installing the askimet plugin

```bash
cd /var/discourse
vi containers/app.yml
```

Update the hooks section:

```yml
hooks:
  after_code:
    - exec:
        cd: $home/plugins
        cmd:
          - git clone https://github.com/discourse/docker_manager.git
    - exec:
        cd: $home/plugins
        cmd:
          - git clone https://github.com/discourse/discourse-akismet.git
```

```bash
./launcher bootstrap app
dokku ps:stop discourse
docker tag local_discourse/app:latest dokku/app-name:latest
dokku tags:deploy app-name latest
```

## Updating

```bash
cd /var/discourse/
./launcher bootstrap app
dokku ps:stop discourse
docker tag local_discourse/app:latest dokku/discourse:latest
dokku tags:deploy discourse latest
```
