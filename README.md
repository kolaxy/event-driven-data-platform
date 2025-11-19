# Event-Driven Data Platform (Local Docker Stack)

This documentation provides a complete cheat sheet for accessing, managing, and connecting to all services in your local Docker Compose stack.

---

## 1. Overview

This stack includes a full event-driven data platform with the following components:

- **Messaging & Streaming**: Kafka, Zookeeper, Schema Registry, Kafka UI  
- **Object Storage**: MinIO  
- **Caching & Broker**: Redis  
- **Workflow Orchestration**: Airflow (CeleryExecutor with Redis + PostgreSQL backend)  
- **Databases**: PostgreSQL, Greenplum, ClickHouse  
- **Visualization**: Superset  
- **Management UI**: pgAdmin for Greenplum  

All services are connected via a dedicated Docker network: `data-platform-network`.

---

## 2. Service Ports & Credentials

| Service | Local Port | Access Type | Username | Password | Notes |
|--------|------------|------------|----------|----------|-------|
| **Zookeeper** | 2181 | TCP | — | — | Internal for Kafka |
| **Kafka** | 9092 | TCP (PLAINTEXT) | — | — | External & internal connections |
| **Schema Registry** | 8081 | HTTP API | — | — | [API docs](http://localhost:8081/subjects) |
| **Kafka UI** | 7777 | Web UI | — | — | [UI](http://localhost:7777) |
| **MinIO Console** | 9006 | Web UI | `minio` | `minio123` | [Console](http://localhost:9006) |
| **MinIO API (S3)** | 9005 | S3-compatible | `minio` | `minio123` | Use `aws-cli` or SDK |
| **Redis** | 6379 | TCP | default | `redis123` | Requires password for CLI |
| **PostgreSQL (Airflow)** | 5432 | TCP | `airflow` | `airflow` | SQL backend for Airflow |
| **Airflow Webserver** | 8085 | Web UI | `admin` | `admin` | [UI](http://localhost:8085) |
| **Greenplum** | 5433 | PostgreSQL-compatible | `greenplum_user` | `greenplum_password` | Multi-node ready (PXF included) |
| **pgAdmin** | 5050 | Web UI | `admin@admin.com` | `admin` | [UI](http://localhost:5050) |
| **ClickHouse HTTP** | 8123 | HTTP API | `default` | `clickhouse123` | Use for queries via HTTP |
| **ClickHouse Native** | 9000 | TCP client | `default` | `clickhouse123` | Native client connections |
| **Superset** | 8088 | Web UI | `admin` | `admin` | [UI](http://localhost:8088) |

> **Tip:** All credentials are set explicitly in `docker-compose.yml`. No `.env` files are used.

---

## 3. Quick Access Links

| Service | URL / Command |
|--------|---------------|
| **Airflow Web UI** | [http://localhost:8085](http://localhost:8085) |
| **Kafka UI** | [http://localhost:7777](http://localhost:7777) |
| **MinIO Console** | [http://localhost:9006](http://localhost:9006) |
| **MinIO API (S3)** | `http://localhost:9005` |
| **pgAdmin** | [http://localhost:5050](http://localhost:5050) |
| **Superset** | [http://localhost:8088](http://localhost:8088) |
| **ClickHouse HTTP API** | [http://localhost:8123](http://localhost:8123) |
| **Schema Registry** | [http://localhost:8081/subjects](http://localhost:8081/subjects) |
| **Zookeeper CLI** | `telnet localhost 2181` → `stat` |
| **Redis CLI** | `redis-cli -h localhost -p 6379 -a redis123` |
| **ClickHouse Client** | `clickhouse client --host localhost --port 9000 --user default --password clickhouse123` |

---

## 4. Docker Volumes

| Volume | Purpose |
|--------|---------|
| `zookeeper_data` | Persistent Zookeeper data |
| `zookeeper_log` | Zookeeper logs |
| `kafka_data` | Kafka message logs |
| `minio_data` | MinIO object storage |
| `redis_data` | Redis persistent storage |
| `postgres_airflow_data` | Airflow PostgreSQL DB |
| `airflow_dags` | DAG definitions |
| `airflow_plugins` | Airflow plugins |
| `greenplum_data` | Greenplum persistent storage |
| `clickhouse_data` | ClickHouse DB data |
| `clickhouse_log` | ClickHouse logs |
| `superset_data` | Superset metadata & configuration |

