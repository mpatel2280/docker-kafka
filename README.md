# Kafka on Docker for development

## Setting up
There are 2 `docker-compose` files.

To spin up kafka + zookeeper
```bash
$ docker-compose up -d
```

To spin up clickhouse
```bash
$ docker-compose -f clickhouse.docker-compose.yml up -d
```

To execute SQL statements in clickhouse, you can exec into clickhouse for the `clickhouse-client` CLI
```
$ docker-compose exec clickhouse clickhouse-client
ClickHouse client version 22.11.2.30 (official build).
Connecting to localhost:9000 as user default.
Connected to ClickHouse server version 22.11.2 revision 54460.

Warnings:
 * Linux is not using a fast clock source. Performance can be degraded. Check /sys/devices/system/clocksource/clocksource0/current_clocksource

4689032967e7 :) 
```
Enter the SQL statements one by one from `clickhouse.sql`.

## Debugging
To delete a topic `posts` from kafka (in case you have published corrupt messages in development to the topic)
```bash
$ docker-compose exec kafka /opt/bitnami/kafka/bin/kafka-topics.sh --bootstrap-server localhost:9092 --delete --topic posts
```

To view the clickhouse logs to check if kafka ingestion is healthy, you can run
```bash
$ docker-compose exec clickhouse tail -f /var/log/clickhouse-server/clickhouse-server.log
```