## Analytic-container
### Hot path analytic container for IoT scenarios


## Versions

* Docker Image:      2.3.0
* Ubuntu:            18.04
* InfluxDB:          1.7.3
* Telegraf (StatsD): 1.9.4-1
* Grafana:           6.0.0
* Cronograf          1.7.8
* Node-red           0.20.5

## Quick Start

To start the container the first time launch:

```sh
docker run --ulimit nofile=66000:66000 \
  -d \
  --name docker-cronograf-influxdb-grafana-node-red \
  -p 3003:3003 \
  -p 3004:8888 \
  -p 8086:8086 \
  -p 8125:8125/udp \
  -p 1880:1880 \
  25987908/hotpathanalytic-it-informatik:v2
```

You can replace `latest` with the desired version listed in changelog file.

To stop the container launch:

```sh
docker stop docker-cronograf-influxdb-grafana-node-red
```

To start the container again launch:

```sh
docker start docker-cronograf-influxdb-grafana-node-red
```

## Mapped Ports

```
Host    Container   Service

3003    3003      grafana
3004    8888      influxdb-admin (chronograf)
8086    8086      influxdb
8125    8125      statsd
1880    1880      Node-red
```

## Grafana

Open <http://localhost:3003>

```
Username: root
Password: root
```

### Add data source on Grafana

1. Using the wizard click on `Add data source`
2. Choose a `name` for the source and flag it as `Default`
3. Choose `InfluxDB` as `type`
4. Choose `direct` as `access`
5. Fill remaining fields as follows and click on `Add` without altering other fields

```
Url: http://localhost:8086
Database: telegraf
User: telegraf
Password: telegraf
```

Basic auth and credentials must be left unflagged. Proxy is not required.

Now you are ready to add your first dashboard and launch some query on database.

## InfluxDB

### Web Interface

Open <http://localhost:3004>

```
Username: root
Password: root
Port: 8086
```

### InfluxDB Shell (CLI)

1. Attach to docker container, run shell `/bin/bash`
2. Launch `influx` to open InfluxDB Shell (CLI)

