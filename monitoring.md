# dokku-monitoring

- Time series data is stored in graphite
- Statsd is used as an interface to populate graphite (your apps submit metrics to statsd)
- Grafana is used to visualize the data

## Install graphite, statsd & grafana

```bash
dokku plugin:install https://github.com/dokku/dokku-graphite.git graphite
dokku graphite:create graphite-master
dokku graphite:expose graphite-master
dokku graphite:info graphite-master
```

(Note the graphite UI port is not exposed and thus not accessible.)

## Install cadvisor

Setup `cadvisor` to monitor & capture container metrics (via statsd):

(Note, `cadvisor` seems to use a lot of CPU, and I have since removed it.)

```bash
VERSION=v0.36.0
docker run \
  --volume=/:/rootfs:ro \
  --volume=/var/run:/var/run:rw \
  --volume=/sys:/sys:ro \
  --volume=/var/lib/docker/:/var/lib/docker:ro \
  --publish=8080:8080 \
  --detach=true \
  --name=cadvisor \
  --link=dokku.graphite.graphite-master.ambassador \
  gcr.io/cadvisor/cadvisor:$VERSION
```

```bash
curl localhost:8080/metrics | grep app-name
```

Access `cadvsisor` dashboard: http://dokku.me:8080/

```bash
nc -vz dokku.graphite.graphite-master.ambassador 8126 # TCP
nc -vzu dokku.graphite.graphite-master.ambassador 8125 # UDP
```

Start & link the container to graphite, using docker entrypoint magic to pass the linked graphite (statsd) ENV variables to the run script.

```bash
docker run \
  --detach=true \
  --volume=/:/rootfs:ro \
  --volume=/var/run:/var/run:rw \
  --volume=/sys:/sys:ro \
  --volume=/var/lib/docker/:/var/lib/docker:ro \
  --publish=8080:8080 \
  --name=cadvisor \
  --link=dokku.graphite.graphite-master \
  --entrypoint /bin/sh gcr.io/cadvisor/cadvisor:$VERSION \
  -c '/usr/bin/cadvisor  -storage_driver=statsd  -storage_driver_host=$(printenv DOKKU.GRAPHITE.GRAPHITE_MASTER_PORT_8125_UDP_ADDR):$(printenv DOKKU.GRAPHITE.GRAPHITE_MASTER_PORT_8125_UDP_PORT)'
```
