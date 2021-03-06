# TIG Stack Example with Cisco Model Driven Telemetry

This repository is an example Telegraf, InfluxDB, and Grafana (TIG) stack which can be used for testing and validation. Enjoy!

### PreRequisites

1. docker or docker desktop must be installed
2. access to the internet for the initial container pull

### Getting Started

1. add docker volumes during initial setup

- `docker volume create --name=grafana-volume`
- `docker volume create --name=influxdb-volume`

2. optionally set docker to swarm mode `docker swarm init`
3. rename `.env.example` to `.env`
4. set passwords for .env file for initial database
5. start the containers: `docker-compose up -d`
6. login to grafana http://localhost:3000 (`admin:admin`)
7. authentication: admin:Welcome1
8. setup database

- db: telegraf
- dbuser: telegraf
- dbpassword: telegraf

- http://influxdb:8086
- basic auth - checked
- username / password for
- HTTP Method: GET

7. save!
8. setup your dashboards

### References

- reference: [cisco blog](https://blogs.cisco.com/developer/getting-started-with-model-driven-telemetry)
  https://github.com/jeremycohoe/cisco-ios-xe-mdt
- reference: [container TIG stack](https://dev.to/project42/install-grafana-influxdb-telegraf-using-docker-compose-56e9)

### Troubleshooting:

> `docker-compose logs -f`

## Cisco Expressway metrics using collectd

Telegraf config file is set to support collectd, just point Expressway to your TIG stack based on reference below

- reference: ![Expressway screenshot 1](/media/enable_cdr-expressway_1.png)
- reference: ![Expressway metrics](/media/expressway-metrics.png)

### Telegraf config for CollectD

```

############## Collectd input from Expressway ############
[[inputs.socket_listener]]
service_address = "udp://:25826"

## Data format to consume.
## Each data format has its own unique set of configuration options, read
## more about them here:
##   https://github.com/influxdata/telegraf/blob/master/docs/DATA_FORMATS_INPUT.md
data_format = "collectd"

## Authentication file for cryptographic security levels
#collectd_auth_file = "/etc/telegraf/collectd/auth_file"
## One of none (default), sign, or encrypt
collectd_security_level = "none"
## Path of to TypesDB specifications
#collectd_typesdb = ["/etc/telegraf/collectd/types.db"]

## Multi-value plugins can be handled two ways.
## "split" will parse and store the multi-value plugin data into separate measurements
## "join" will parse and store the multi-value plugin as a single multi-value measurement.
## "split" is the default behavior for backward compatability with previous versions of influxdb.
collectd_parse_multivalue = "join"

```
