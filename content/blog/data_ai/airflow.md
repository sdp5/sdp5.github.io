---
title: Building Data Pipelines Using Apache Airflow
type: blog
date: 2025-03-09 20:34:10 +0530
prev: blog/data_ai/
sidebar:
  open: true

---

As organizations increasingly rely on data to drive decisions, data pipelines—systems that automate the flow of data from source to destination—have become mission-critical. And when it comes to orchestrating these pipelines reliably, Apache Airflow is one of the most powerful tools in the data engineer’s toolkit.

<!--more-->

In this blog post, we'll explore how to build scalable, modular, and production-ready data pipelines using Apache Airflow. We'll use the [Data Pipelines with Airflow](https://github.com/sdp5/data-pipelines-with-airflow) project as our base and highlight key architectural and engineering patterns drawn from it.

## Why Airflow?

Airflow lets you **programmatically author, schedule, and monitor workflows** using Python. It is especially suitable for:

- **ETL orchestration**
- **Event-driven workflows**
- **Data quality enforcement**
- **Cloud integration (e.g., AWS)**

Its power lies in how easily you can define complex dependencies between tasks, monitor DAG execution visually, and plug into a rich ecosystem of providers and hooks.

## Pipeline Components Overview

Let’s break down the essential components of a well-designed Airflow-based data pipeline:

### 1. Directed Acyclic Graphs (DAGs)

A DAG defines the pipeline: it determines **when**, **what**, and **in what** order things should run.

From the repo’s `final_project.py`:

```bash
from airflow import DAG
from airflow.operators.dummy_operator import DummyOperator
from datetime import datetime, timedelta

default_args = {
    'owner': 'airflow',
    'start_date': datetime(2021, 1, 1),
    'retries': 3,
    'retry_delay': timedelta(minutes=5),
    'catchup': False,
    'email_on_retry': False
}

dag = DAG('sparkify_etl_dag',
          default_args=default_args,
          description='Load and transform data in Redshift with Airflow',
          schedule_interval='@hourly'
)
```

### 2. Custom Operators

Operators define what to do. In this repo, the following custom operators are implemented in `plugins/operators`:

- `StageToRedshiftOperator`
- `LoadFactOperator`
- `LoadDimensionOperator`
- `DataQualityOperator`

These encapsulate logic like:

- Running Redshift `COPY` commands from S3
- Loading dimension and fact tables
- Running SQL checks for data quality

Example from `StageToRedshiftOperator`:

```bash
copy_sql = f"""
COPY {self.table}
FROM '{s3_path}'
CREDENTIALS 'aws_iam_role={role_arn}'
FORMAT AS JSON '{self.json_path}'
REGION '{self.region}'
"""
```

### 3. Data Flow

As shown in the DAG, data flows in this sequence:

```bash
Begin Execution
   ↓
Stage Events     Stage Songs
   ↓                 ↓
     Load Fact Table
           ↓
Load Dimension Tables
           ↓
Run Data Quality Checks
           ↓
    End Execution
```

## Parameterization & Templating

From PROJECT.md, the pipeline emphasizes parameterization and templated logic, making it easy to:

- Run backfills
- Reuse operators
- Adapt to changes in data sources

Example: S3 keys use Jinja templates to dynamically include execution date:

```bash
s3_key='log_data/{{ execution_date.year }}/{{ execution_date.month }}'
```

## Integrating with AWS

The pipeline connects to:

- **S3** (as the raw data store)
- **Redshift** (as the data warehouse)

To set this up:

1. Create an IAM Role with access to S3 and Redshift
2. Add Connections in Airflow UI:
   - `aws_credentials` with Access Key/Secret
   - `redshift` as a Postgres-type connection

```bash
{
  "Conn Id": "aws_credentials",
  "Conn Type": "Amazon Web Services",
  "Login": "<AWS_ACCESS_KEY>",
  "Password": "<AWS_SECRET_KEY>"
}
```

Redshift:

```bash
{
  "Conn Id": "redshift",
  "Conn Type": "Postgres",
  "Host": "<cluster-endpoint>",
  "Schema": "dev",
  "Login": "awsuser",
  "Password": "<your-password>",
  "Port": 5439
}
```

## Data Quality Checks

An often neglected but crucial part of any data pipeline is **data validation**.

The `DataQualityOperator` runs assertions like:

- Row count > 0
- No nulls in primary key columns
- Custom SQL tests

From `PROJECT.md`:

> “They have also noted that data quality plays a big part … and want to run tests after the ETL steps to catch discrepancies…”

Sample check:

```bash
dq_checks = [
    {'sql': "SELECT COUNT(*) FROM users WHERE userid IS NULL", 'expected_result': 0},
    {'sql': "SELECT COUNT(*) FROM songs", 'expected_result': lambda x: x > 0}
]
```

## Tracking Lineage

Lineage is about knowing where your data came from and what transformations it has undergone.

Airflow supports lineage using:

- **Graph View** – shows upstream/downstream dependencies
- **XComs** – passing metadata between tasks
- **Naming conventions** – using meaningful DAG and task IDs

You can even extend DAGs to push lineage metadata to systems like OpenLineage.

From LINEAGE.md:

```bash
@dag(
    start_date=pendulum.now()
)
def data_lineage():


    @task()
    def load_trip_data_to_redshift():
        metastoreBackend = MetastoreBackend()
        aws_connection=metastoreBackend.get_connection("aws_credentials")
        redshift_hook = PostgresHook("redshift")
        sql_stmt = sql_statements.COPY_ALL_TRIPS_SQL.format(
            aws_connection.login,
            aws_connection.password,
        )
        redshift_hook.run(sql_stmt)

    load_trip_data_to_redshift_task = load_trip_data_to_redshift()

    @task()
    def load_station_data_to_redshift():
        metastoreBackend = MetastoreBackend()
        aws_connection=metastoreBackend.get_connection("aws_credentials")
        redshift_hook = PostgresHook("redshift")
        sql_stmt = sql_statements.COPY_STATIONS_SQL.format(
            aws_connection.login,
            aws_connection.password,
        )
        redshift_hook.run(sql_stmt)

    load_station_data_to_redshift_task = load_station_data_to_redshift()

    create_trips_table = PostgresOperator(
        task_id="create_trips_table",
        postgres_conn_id="redshift",
        sql=sql_statements.CREATE_TRIPS_TABLE_SQL
    )


    create_stations_table = PostgresOperator(
        task_id="create_stations_table",
        postgres_conn_id="redshift",
        sql=sql_statements.CREATE_STATIONS_TABLE_SQL,
    )


    calculate_traffic_task = PostgresOperator(
        task_id='calculate_location_traffic',
        postgres_conn_id="redshift",
        sql=sql_statements.LOCATION_TRAFFIC_SQL,
    )

    create_trips_table >> load_trip_data_to_redshift_task >> calculate_traffic_task
    create_stations_table >> load_station_data_to_redshift_task

data_lineage_dag = data_lineage()
```

## Monitoring Pipelines

Monitoring is critical in production pipelines. Airflow offers:

- **UI dashboards**
- **SLA miss notifications**
- **Retries and alerting**
- **Logs per task execution**

From `MONITOR.md`:

```bash
- Use Airflow web UI to track DAG runs and task status.
- Enable email alerts for task failures.
- Configure retries and retry delays in default_args.
- Leverage task logs for debugging issues.
```

You can also integrate tools like:

- **PagerDuty**, **Slack alerts**
- **Prometheus**/**Grafana** for metrics

## Best Practices Recap

| Practice                    | Why It Matters                     |
| --------------------------- | ---------------------------------- |
| Use custom operators        | Reusable and testable              |
| Parameterize DAGs           | Enables backfills, dynamic configs |
| Include data quality checks | Prevents silent failures           |
| Monitor via UI + alerts     | Catch and respond to issues fast   |
| Use templated fields        | Dynamic execution control          |
| Document lineage            | Maintain auditability              |

## Final Thoughts

Apache Airflow gives data engineers a powerful platform to orchestrate complex workflows. When used with cloud services like AWS S3 and Redshift, it forms the backbone of reliable, scalable data infrastructure.

This tutorial walked you through the key steps of **building your own data pipeline**:

- Define a DAG and set dependencies
- Build modular operators
- Parameterize with templates
- Run on AWS
- Monitor and validate

Want to try it yourself? Clone the repo and start building!

👉 https://github.com/sdp5/data-pipelines-with-airflow
