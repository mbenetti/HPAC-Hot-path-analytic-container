## HPAC
### Hot-path analytic container for IoT scenarios
<p align="center">
  <img width="600" height="300" src="https://i1.wp.com/codeblog.dotsandbrackets.com/wp-content/uploads/2017/01/grafana-dashboard.jpg?resize=598%2C306">
</p>

This repository was possible thanks to the work of many people before me: https://github.com/samuelebistoletti/docker-statsd-influxdb-grafana , https://github.com/philhawthorne/docker-influxdb-grafana , https://gist.github.com/xoseperez/e23334910fb45b0424b35c422760cb87 , https://www.home-assistant.io/blog/2017/04/25/influxdb-grafana-docker/ , https://medium.com/@petey5000/monitoring-your-home-network-with-influxdb-on-raspberry-pi-with-docker-78a23559ffea .

For the optimal performance of the system the container should run in a minimun of 2-core x86 architecture with at least 2 GB of RAM and a SSD (solid state drive, influxDB requirement). The idea is a solution for dashboarding analytics and real-time sensor data in IoT scenarios. The diference of this container respect with the rest is the addition of node-red. This last layer is used for ingestion (ETL) of the IoT data into InfluxDB for later analisis (OLAP).

## Versions use for the container

* Docker Image:      2.3.0
* Ubuntu:            18.04
* InfluxDB:          1.7.3
* Telegraf (StatsD): 1.9.4-1
* Grafana:           6.0.0
* Cronograf          1.7.8
* Node-red           0.20.5


![HPAC](https://user-images.githubusercontent.com/27162948/59157187-9590a680-8aa6-11e9-865c-3e65b5b69416.JPG)
  
This is the architecture of the container and the different layers of abstraction

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

### Add the DB on Grafana

1. Using the wizard click on `Add data source`
2. Choose a `name` for the source and flag it as `Default`
3. Choose `InfluxDB` as `type`
4. Choose `direct` as `access`
5. Fill remaining fields as follows and click on `Add` without user and password

```
Url: http://localhost:8086
Database: telegraf
User: telegraf
Password: telegraf
```

Basic auth and credentials must be left unflagged. Proxy is not required.

Now you are ready to add your first dashboard and launch some query on database. Out of the box the container provide a dashboard with information of the resources consume by the container in the host machine.

## InfluxDB

### Web Interface for administration

Open <http://localhost:3004>

```
1 - Get started
2 - Add a connection, use by default "telegraf" no pass no user
3 - Add a "System" dashboard, press "Create 1 dashboard"
4 - Kapacitor, this step skip it.
5 - Done, go to view all connections
```
Only use in case of adminstration of the databases. Is not use in a normal operation, every new message sent to the DB with a new topic creates automatically a new database with new measurements. In dashboard tap should already a dashboard of the host system. 

### InfluxDB Shell (CLI)

1. Attach to docker container, run shell `/bin/bash`
2. Launch `influx` to open InfluxDB Shell (CLI)

## Node-red

Open <http://localhost:1880>

```
Username: 
Password: 
```
You can create a user and assing privileges.
