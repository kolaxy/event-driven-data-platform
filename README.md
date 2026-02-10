# Event-Driven Data Platform (Local Docker Stack)

Complete local stack for developing and testing an event-driven data platform on Docker.

## 1. Overview

The stack includes:

* **Messaging & Streaming**: Kafka, Zookeeper, Schema Registry, Kafka UI
* **Object Storage**: MinIO
* **Caching & Broker**: Redis
* **Workflow Orchestration**: Airflow (CeleryExecutor with Redis + PostgreSQL backend)
* **Databases**: PostgreSQL, Greenplum, ClickHouse, Oracle XE
* **Visualization**: Superset
* **Monitoring & Management**: Dozzle, Portainer
* All services are connected through the Docker network: `data-platform-network`.

## 2. System Requirements

* Docker >= 20.10
* Docker Compose >= 2.0
* Minimum 16GB RAM for stable operation of all services

## 3. Start the Stack

```bash
docker-compose up -d
```

Check service status:

```bash
docker-compose ps
docker-compose logs -f <service_name>
```

Stop the stack:

```bash
docker-compose down
```

## 4. Services, Ports, and Credentials

| Service              | Local Port | Access     | Username       | Password           | Notes                                          |
| -------------------- | ---------- | ---------- | -------------- | ------------------ | ---------------------------------------------- |
| Zookeeper            | 2181       | TCP        | —              | —                  | Internal for Kafka                             |
| Kafka                | 9092       | TCP        | —              | —                  | PLAINTEXT connections                          |
| Schema Registry      | 8081       | HTTP API   | —              | —                  | [API Docs](http://localhost:8081/subjects)     |
| Kafka UI             | 7777       | Web UI     | —              | —                  | [UI](http://localhost:7777)                    |
| MinIO Console        | 9006       | Web UI     | minio          | minio123           | [Console](http://localhost:9006)               |
| MinIO S3 API         | 9005       | S3         | minio          | minio123           | For SDK or aws-cli                             |
| Redis                | 6379       | TCP        | —              | redis123           | Password required                              |
| PostgreSQL (Airflow) | 5432       | TCP        | airflow        | airflow            | SQL backend for Airflow                        |
| Airflow Web          | 8085       | Web UI     | admin          | admin              | [UI](http://localhost:8085)                    |
| Greenplum            | 5433       | PostgreSQL | greenplum_user | greenplum_password | Multi-node ready                               |
| ClickHouse HTTP      | 8123       | HTTP API   | default        | clickhouse123      | `?user=default&password=clickhouse123`         |
| ClickHouse TCP       | 9000       | TCP client | default        | clickhouse123      | Native client                                  |
| Oracle XE            | 1521       | TCP (TNS)  | system/sys     | oracle_password    | SID: `XE`                                      |
| Superset             | 8088       | Web UI     | admin          | admin              | [UI](http://localhost:8088)                    |
| Pinot Controller     | 9002       | Web UI     | —              | —                  | [UI](http://localhost:9002)                    |
| Pinot Broker         | 8000       | TCP        | —              | —                  | —                                              |
| Pinot Server         | 8001       | TCP        | —              | —                  | —                                              |
| Dozzle               | 8089       | Web UI     | —              | —                  | [View Docker Logs](http://localhost:8089)      |
| Portainer            | 8066       | Web UI     | —              | —                  | [Docker Management GUI](http://localhost:8066) |

## 5. Quick Access Commands

```bash
# Kafka
echo stat | nc localhost 2181

# Redis
redis-cli -h localhost -p 6379 -a redis123

# ClickHouse
clickhouse client --host localhost --port 9000 --user default --password clickhouse123

# Oracle SQL*Plus inside container
docker exec -it oracle_xe_local sqlplus system/oracle_password@//localhost:1521/XE
```

## 6. Docker Volumes

| Volume                           | Description                               |
| -------------------------------- | ----------------------------------------- |
| zookeeper_data / zookeeper_log   | Zookeeper data and logs                   |
| kafka_data                       | Kafka message logs                        |
| minio_data                       | MinIO object storage                      |
| redis_data                       | Redis data                                |
| postgres_airflow_data            | PostgreSQL for Airflow                    |
| airflow_dags / airflow_plugins   | Airflow DAGs and plugins                  |
| greenplum_data                   | Greenplum data                            |
| clickhouse_data / clickhouse_log | ClickHouse data and logs                  |
| oracle_data                      | Oracle XE database files                  |
| superset_data                    | Superset configuration and metadata       |
| pinot_*                          | Pinot Controller, Broker, and Server data |
| portainer_data                   | Portainer data                            |

## 7. Useful Tips

* For Airflow Web UI, wait for the database and Redis to be ready (`docker-compose logs airflow-webserver`).
* Superset and Airflow automatically create the `admin` user on first launch.
* Kafka UI connects to Kafka using the internal container DNS (`kafka:9092`).
* Dozzle allows you to view logs for all containers via [http://localhost:8089](http://localhost:8089).
* Portainer provides a web GUI for managing Docker at [http://localhost:8066](http://localhost:8066).
* To reset all data, run `docker-compose down -v`.
