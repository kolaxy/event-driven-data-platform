# Local Data Platform - Quick Cheat Sheet

This cheat sheet summarizes ports, credentials, and access points for all services in your local Docker Compose stack.

---

## Services, Ports & Credentials

| Service | Local Port | Internal Port | Default User / Password / DB |
|---------|------------|---------------|------------------------------|
| **Zookeeper** | 2181 | 2181 | — |
| **Kafka** | 9092 (PLAINTEXT_L), 9093 (PLAINTEXT_C) | 29092 (PLAINTEXT_INTERNAL) | — |
| **Schema Registry** | 8081 | 8081 | — |
| **Kafka UI** | 7777 → web UI | — | — |
| **MinIO** | 9005 → API, 9006 → Console | — | `minio / minio123` |
| **Redis** | 6379 | 6379 | — (no password) |
| **PostgreSQL (Airflow)** | 5432 | 5432 | `airflow / airflow / airflow` |
| **Airflow Webserver** | 8085 | 8080 | Admin account: `admin / admin` |
| **Greenplum** | 5433 → mapped to 5432 | 5432 | `greenplum_user / greenplum_password / greenplum_db` |
| **pgAdmin** | 5050 → web UI | — | `admin@admin.com / admin` |
| **ClickHouse** | 8123 → HTTP, 9000 → Native | 9000 | `default / <empty>` |
| **Superset** | 8088 | 8088 | Create admin via CLI: `docker exec -it superset_local superset fab create-admin` |

---

## Quick Notes

- **Kafka Listeners**
  - `PLAINTEXT_L` → localhost:9092 (external)
  - `PLAINTEXT_C` → kafka:9093 (internal)
  - `PLAINTEXT_INTERNAL` → kafka:29092 (inter-broker)
- **Airflow**
  - Uses CeleryExecutor
  - Scheduler runs in the same container as webserver
  - PostgreSQL connection: `postgres_airflow:5432`
  - Redis broker: `redis:6379/0`
- **MinIO**
  - Console UI: `http://localhost:9006`
  - API: `http://localhost:9005`
- **Greenplum & pgAdmin**
  - Connect pgAdmin to Greenplum using `greenplum_local:5432`
- **ClickHouse**
  - HTTP: `http://localhost:8123`
  - Native client: `localhost:9000`
- **Superset**
  - Web UI: `http://localhost:8088`

---

## Docker Volumes

| Volume | Purpose |
|--------|---------|
| `zookeeper_data` | Zookeeper data |
| `zookeeper_log` | Zookeeper logs |
| `kafka_data` | Kafka data |
| `minio_data` | MinIO data |
| `redis_data` | Redis data |
| `postgres_airflow_data` | PostgreSQL (Airflow) data |
| `airflow_dags` | DAGs |
| `airflow_plugins` | Airflow plugins |
| `greenplum_data` | Greenplum data |
| `clickhouse_data` | ClickHouse data |
| `clickhouse_log` | ClickHouse logs |
| `superset_data` | Superset home data |
