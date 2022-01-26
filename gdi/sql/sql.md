(sql)=

# SQL

<meta name="description" content="Documentation on the SQL monitor">

## Description

In the {ref}`Splunk Distribution of OpenTelemetry Collector <otel-intro>`, this integration is the
`sql` monitor in the Smart Agent Receiver. Use this monitor to gather database usage metrics from SQL queries on your databases.

##  Install the monitor

This monitor is available in the SignalFx Smart Agent Receiver, which is part of the {ref}`Splunk Distribution of OpenTelemetry Collector <otel-intro>`.

To install this integration:

1. Deploy the Splunk Distribution of OpenTelemetry Collector to your host or container platform.

2. Configure the monitor, as described in the next section.

## Configure the monitor

This Splunk Distribution of OpenTelemetry Collector allows embedding a Smart Agent monitor configuration in an associated Smart Agent Receiver instance.

**Note:** Providing an SQL monitor entry in your Smart Agent or Collector configuration is required for its use. Use the appropriate form for your agent type.

### Configure the monitor for Smart Agent

To activate this monitor in the Smart Agent, add the following to your agent configuration:

```yaml
monitors:  # All monitor config goes under this key
 - type: sql
   ...  
   # Additional config
```

See the <a href="https://docs.splunk.com/Observability/gdi/smart-agent/smart-agent-resources.html#configure-the-smart-agent" target="_blank">Smart Agent example configuration</a> for a generated example of a YAML configuration file, with default values where applicable.

### Configure the monitor for Splunk Distribution of OpenTelemetry Collector

To activate this monitor in the Splunk Distribution of OpenTelemetry Collector, add the following to your agent configuration:

```yaml
receivers:
  smartagent/sql:
    type: sql
    ...  
    # Additional config
```

See <a href="https://github.com/signalfx/splunk-otel-collector/tree/main/examples" target="_blank">configuration examples</a> for specific use cases that show how the Splunk OpenTelemetry Collector can integrate and complement existing environments.

**Note:** Monitors with <a href="https://dev.splunk.com/observability/docs/datamodel/#Creating-or-updating-custom-properties-and-tags" target="_blank">dimension property and tag update functionality</a> provide an associated `dimensionClients` field that references the name of the exporter you're using in your pipeline. If you don't specify any exporters using this field, the Smart Agent Receiver tries to use the associated pipeline. If the next element of the pipeline isn't compatible with the dimension update behavior, and you configured a single exporter, the system selects the exporter. If you don't want dimension updates, specify an empty array (`[]`) to disable the feature.

### Configuration settings 

The following tables show the configuration options for this monitor:

| **Option**         | **Required** | **Type**                      | **Description**                                                                                                                                                                                                                                                                                                    |
|:-------------------|:-------------|:------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `queries`          | **yes**      | `list of objects (see below)` | A list of queries that generate data points                                                                                                                                                                                                                                                                        |
| `host`             | no           | `string`                      |                                                                                                                                                                                                                                                                                                                    |
| `port`             | no           | `integer`                     | (**default:** `0`)                                                                                                                                                                                                                                                                                                 |
| `params`           | no           | `map of strings`              | Replaceable parameters, in the form of key-value pairs. The system inserts the values into `connectionString` for a specified key, using Go template syntax (for example `{{.key}}` ).                                                                                                                             |
| `dbDriver`         | no           | `string`                      | The database driver to use. Valid values are `postgres`, `mysql`, `sqlserver`, and `snowflake`.                                                                                                                                                                                                                    |
| `connectionString` | no           | `string`                      | Connection string and replaceable parameters used to connect to the database. To learn more, see the list of connection string parameters for the Go `pq` package at [https://godoc.org/github.com/lib/pq#hdr-Connection_String_Parameters](https://godoc.org/github.com/lib/pq#hdr-Connection_String_Parameters). |
| `logQueries`       | no           | `bool`                        | (**default:** `false`) If true, log query results info level.                                                                                                                                                                                                                                                      |

The **nested** `queries` configuration object has the following fields:

| **Option**             | **Required** | **Type**                      | **Description**                                                                                                                                                                                                                                    |
|:-----------------------|:-------------|:------------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `query`                | **yes**      | `string`                      | An SQL query text that selects one or more rows from a database                                                                                                                                                                                    |
| `params`               | no           | `list of values`              | Optional parameters that replace placeholders in the query string.                                                                                                                                                                                 |
| `metrics`              | no           | `list of objects (see below)` | Metrics generated from the query.                                                                                                                                                                                                                  |
| `datapointExpressions` | no           | `list of strings`             | A set of [expr] expressions that convert each row to a set of metrics. Each of these run for each row in the query result set, allowing you to generate multiple data points per row. Each expression must evaluate to a single data point or nil. |

The **nested** `metrics` configuration object has the following fields:

| **Option**                 | **Required** | **Type**          | **Description**                                                                                                                                                                                                                                                                                         |
|:---------------------------|:-------------|:------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `metricName`               | **yes**      | `string`          | The name of the metric as it appears in Splunk Observability Cloud.                                                                                                                                                                                                                                     |
| `valueColumn`              | **yes**      | `string`          | The column name that holds the data point value                                                                                                                                                                                                                                                         |
| `dimensionColumns`         | no           | `list of strings` | The names of the columns that make up the dimensions of the data point.                                                                                                                                                                                                                                 |
| `isCumulative`             | no           | `bool`            | Whether the value is a cumulative counters (true) or gauge (false). If you set this to the wrong value and send in your first data point for the metric name with the wrong type, you have to manually change the type, as it is set in the system based on the first type seen. (**default:** `false`) |
| `dimensionPropertyColumns` | no           | `map of lists`    | The mapping between dimensions and the columns to be used to attach respective properties                                                                                                                                                                                                               |

### Using the monitor

Consider the following `customers` database table:

| **id** | **name**  | **country** | **status** |
|:-------|:----------|:------------|:-----------|
| 1      | Bill      | USA         | active     |
| 2      | Mary      | USA         | inactive   |
| 3      | Joe       | USA         | active     |
| 4      | Elizabeth | Germany     | active     |

Use the following monitor configuration to generate metrics about active users and customer counts by country:

```yaml
monitors:
  - type: sql
    host: localhost
    port: 5432
    dbDriver: postgres
    params:
      user: admin
      password: s3cr3t
    # The `host` and `port` values shown in this example (also provided through autodiscovery) are interpolated
    # to the connection string as appropriate for the database driver.
    # Also, the values from the `params` configuration option above can be
    # interpolated.
    connectionString: 'host={{.host}} port={{.port}} dbname=main user={{.user}} password={{.password}} sslmode=disable'
    queries:
      - query: 'SELECT COUNT(*) as count, country, status FROM customers GROUP BY country, status;'
        metrics:
          - metricName: "customers"
            valueColumn: "count"
            dimensionColumns: ["country", "status"]
```

When you use this configuration, you get series of MTS, all with the metric name `customers`. Each MTS has a `county` and `status` dimension. The dimension value is the number of customers that belong to that combination of `country` and `status`. You can also specify multiple `metrics` items to generate more than one metric from a single query.

### Using metric expressions

**Note:** Metric expressions are a beta feature and might break in subsequent non-major releases, but this example is maintained so that it's backwards compatible.

If you need to do more complex logic than simply mapping columns to metric values and dimensions, use the `datapointExpressions` option that's available for individual metric configurations. Create more sophisticated logic to derive data points from individual rows by using the `expr` expression language. These expressions must evaluate to data points created by the `GAUGE` or `CUMULATIVE` helper functions available in the expression's context. You can also have the expression evaluate to `nil` if you don't need to generate a data point for a particular row.

Both the `GAUGE` and `CUMULATIVE` functions have the following signature:

(`metricName`, `dimensions`, `value`)

 * `metricName`: Must be a string
 * `dimensions`: Must be a map of string keys and values, and
 * `value`: Must be a numeric value.

Each of the columns in the row maps to a variable in the context of the expression with the same name.
For example, if you have a column called `name` in your SQL query result, you can use a variable called `name` in the expression.
In your expression, surround string values with single quotes (`''`).

For example, the MySQL `SHOW REPLICA STATUS` query doesn't let you pre-process columns using SQL,
but you can convert the `Replica_IO_Running` column (a string `Yes/No` value) to a gauge data point with values of value of 0 or 1 by using the following configuration:

```yaml
   - type: sql
     # This is an example discovery rule. Your environment might be different.
     discoveryRule: container_labels["mysql.replica"] == "true" && port == 3306
     dbDriver: mysql
     params:
       user: root
       password: password
     connectionString: '{{.user}}:{{.password}}@tcp({{.host}})/<database>'
     # You can also use '.user:.password@tcp(.host)/' if you don't want to specify a database.
     queries:
      - query: 'SHOW REPLICA STATUS'
        datapointExpressions:
          - 'GAUGE("mysql.replica_sql_running", {main_uuid: Main_UUID, channel: Channel_name}, Replica_SQL_Running == "Yes" ? 1 : 0)'
```

Use this configuration to generate a single gauge data point for each row in the `replica status` output, with two dimension, `main_uuid` and `channel`, and with a value of 0 or 1, depending on if the SQL thread for the replica is running.

### Supported drivers

You must specify the `dbDriver` option that contains the name of the database driver to use.
These names are the same as the name of the Golang SQL driver used in the agent.
The monitor formats the `connectionString` according to the driver you specify. The following list shows you the currently-supported drivers:

- `postgres`
- `mysql`
- `sqlserver`
- `snowflake`
- `hana`

### Parameterized connection string

The monitor treats the value of `connectionString` as a Golang template with a context consisting of the variables `host` and `port` and all the parameters from
the `params` option. To add a variable to the template, use the Golang `{{.varname}}` template syntax.

### Collect Snowflake performance and usage metrics

To configure the agents to collect Snowflake performance and usage metrics, perform the following steps:

1. Copy the `pkg/sql/snowflake-metrics.yaml` file from the `sql` monitor repo into the same location as your `agent.yaml` file (for example, `/etc/splunk`).
2. Configure the SQL monitor as follows:

```yaml
monitors:
  - type: sql
    intervalSeconds: 3600
    dbDriver: snowflake
    params:
      account: "account.region"
      database: "SNOWFLAKE"
      schema: "ACCOUNT_USAGE"
      role: "ACCOUNTADMIN"
      user: "user"
      password: "password"
    connectionString: "{{.user}}:{{.password}}@{{.account}}/{{.database}}/{{.schema}}?role={{.role}}"
    queries:
      {"#from": "/etc/signalfx/snowflake-metrics.yaml"}
```

If you want, you can also copy the contents of `snowflake-metrics.yaml` into `agent.yaml` under `queries`. Edit `snowflake-metrics.yaml` to only include the metrics you want to monitor.

## Metrics filtering

The Smart Agent and Splunk Distribution of OpenTelemetry Collector doesn't do any built-in filtering of metrics coming out of this monitor.

## Troubleshooting

```{include} /_includes/troubleshooting.md
```