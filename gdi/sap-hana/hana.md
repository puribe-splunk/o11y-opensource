(hana)=

# SAP HANA

Monitor Type: `hana` ([Source](https://github.com/signalfx/signalfx-agent/tree/main/pkg/monitors/hana))

**Accepts Endpoints**: **Yes**

**Multiple Instances Allowed**: Yes

## Description

This monitor sends SQL queries to an SAP HANA database, emitting the results as metrics.

```yaml
monitors:
  - type: hana
    host: myhost.hana.us.hanacloud.ondemand.com
    port: 443
    username: SOMEUSER
    password: s3cr3t
```


## Configure the SAP HANA monitor

These monitors are available in the SignalFx Smart Agent Receiver, which is part of the {ref}`Splunk Distribution of OpenTelemetry Collector <otel-intro>`.

To activate this monitor in the Smart Agent, add the following to your agent configuration:

```
monitors:  # All monitor config goes under this key
 - type: hana
   ...  # Additional config
```

**For a list of monitor options that are common to all monitors, see [Common
Configuration](../monitor-config.md#common-configuration).**


| Config option | Required | Type | Description |
| --- | --- | --- | --- |
| `tlsServerName` | no | `string` | ServerName to verify the hostname. Defaults to Host if not specified. |
| `insecureSkipVerify` | no | `bool` | Controls whether a client verifies the server's certificate chain and host name. Defaults to false. (**default:** `false`) |
| `rootCAFiles` | no | `list of strings` | Path to root certificate(s) (optional) |


## Metrics

These are the metrics available for this monitor.
Metrics that are categorized as
[container/host](https://docs.splunk.com/Observability/admin/monitor-imm-billing-usage.html#nav-Monitor-Splunk-Infrastructure-Monitoring-billing-and-usage)
(*default*) are ***in bold and italics*** in the list below.


 - ***`sap.hana.connection.count`*** (*gauge*)<br>    Number of connections

 - ***`sap.hana.connection.message.received.count`*** (*gauge*)<br>    Total count of messages received

 - ***`sap.hana.connection.message.received.size`*** (*gauge*)<br>    Total size of messages received

 - ***`sap.hana.connection.message.sent.count`*** (*gauge*)<br>    Total count of messages sent

 - ***`sap.hana.connection.message.sent.size`*** (*gauge*)<br>    Total size of messages sent

 - ***`sap.hana.connection.record.affected`*** (*gauge*)<br>    The sum of the record count affected by DML/DDL statements

 - ***`sap.hana.connection.record.fetched`*** (*gauge*)<br>    Number of records fetched by select statements

 - ***`sap.hana.disk.total_size`*** (*gauge*)<br>    Total device size returned by the operating system in bytes

 - ***`sap.hana.disk.used_size`*** (*gauge*)<br>    Size of used disk space in bytes

 - ***`sap.hana.host.cpu.idle`*** (*counter*)<br>    CPU idle time in milliseconds

 - ***`sap.hana.host.cpu.system`*** (*counter*)<br>    CPU time spent in kernel mode

 - ***`sap.hana.host.cpu.user`*** (*counter*)<br>    CPU time spent in user mode in milliseconds

 - ***`sap.hana.host.cpu.wio`*** (*counter*)<br>    CPU time spent in wait IO (Linux only, Windows always 0) in milliseconds

 - ***`sap.hana.host.file.open`*** (*gauge*)<br>    Number of open files

 - ***`sap.hana.host.memory.allocation_limit`*** (*gauge*)<br>    Allocation limit for all processes in bytes

 - ***`sap.hana.host.memory.code`*** (*gauge*)<br>    Code size, including shared libraries of instance processes in bytes

 - ***`sap.hana.host.memory.physical.free`*** (*gauge*)<br>    Free physical memory on the host in bytes

 - ***`sap.hana.host.memory.physical.used`*** (*gauge*)<br>    Used physical memory on the host in bytes

 - ***`sap.hana.host.memory.shared`*** (*gauge*)<br>    Shared memory size of instance processes in bytes

 - ***`sap.hana.host.memory.swap.free`*** (*gauge*)<br>    Free swap memory on the host in bytes

 - ***`sap.hana.host.memory.swap.used`*** (*gauge*)<br>    Used swap memory on the host in bytes

 - ***`sap.hana.host.memory.total_allocated`*** (*gauge*)<br>    Size of the memory pool for all instance processes in bytes

 - ***`sap.hana.host.memory.total_used`*** (*gauge*)<br>    Amount of memory from the memory pool that is currently being used by SAP HANA processes in bytes

 - ***`sap.hana.io.append.count`*** (*counter*)<br>    Number of appends

 - ***`sap.hana.io.read.async.count`*** (*counter*)<br>    Number of triggered asynchronous reads

 - ***`sap.hana.io.read.count`*** (*counter*)<br>    Number of synchronous reads

 - ***`sap.hana.io.read.failed`*** (*counter*)<br>    Number of failed reads

 - ***`sap.hana.io.read.size`*** (*counter*)<br>    Size of read data in bytes

 - ***`sap.hana.io.read.time`*** (*counter*)<br>    Total read time in microseconds

 - ***`sap.hana.io.total.time`*** (*counter*)<br>    Total write time in microseconds

 - ***`sap.hana.io.write.async.count`*** (*counter*)<br>    Number of triggered asynchronous writes

 - ***`sap.hana.io.write.count`*** (*counter*)<br>    Number of synchronous writes

 - ***`sap.hana.io.write.failed`*** (*counter*)<br>    Number of failed writes

 - ***`sap.hana.io.write.size`*** (*counter*)<br>    Size of written data in bytes

 - ***`sap.hana.io.write.time`*** (*counter*)<br>    Total write time in microseconds

 - ***`sap.hana.service.component.memory.used`*** (*gauge*)<br>    Amount of memory from the memory pool currently in use by component

 - ***`sap.hana.service.cpu.utilization`*** (*gauge*)<br>    CPU utilization in percent

 - ***`sap.hana.service.file.open`*** (*gauge*)<br>    Number of open files

 - ***`sap.hana.service.memory.allocation_limit`*** (*gauge*)<br>    Maximum configured memory pool size in bytes

 - ***`sap.hana.service.memory.allocation_limit_effective`*** (*gauge*)<br>    Effective maximum memory pool size in bytes, considering the pool sizes of other processes

 - ***`sap.hana.service.memory.code`*** (*gauge*)<br>    Code size, including shared libraries in bytes

 - ***`sap.hana.service.memory.heap.allocated`*** (*gauge*)<br>    Heap part of the memory pool in bytes

 - ***`sap.hana.service.memory.heap.used`*** (*gauge*)<br>    Amount of pool heap memory in use in bytes

 - ***`sap.hana.service.memory.logical`*** (*gauge*)<br>    Virtual memory size in bytes

 - ***`sap.hana.service.memory.physical`*** (*gauge*)<br>    Physical memory size in bytes

 - ***`sap.hana.service.memory.shared.allocated`*** (*gauge*)<br>    Shared memory part of the memory pool in bytes

 - ***`sap.hana.service.memory.shared.used`*** (*gauge*)<br>    Amount of shared memory pool in use in bytes

 - ***`sap.hana.service.memory.stack`*** (*gauge*)<br>    Stack size

 - ***`sap.hana.service.memory.total_used`*** (*gauge*)<br>    Amount of memory in use from the memory pool

 - ***`sap.hana.statement.active.count`*** (*gauge*)<br>    Number of active statements

 - ***`sap.hana.statement.active.execution.memory.max`*** (*gauge*)<br>    Max memory size used during execution

 - ***`sap.hana.statement.active.execution.memory.mean`*** (*gauge*)<br>    Average memory size used during execution

 - `sap.hana.statement.active.execution.memory.sum` (*gauge*)<br>    Sum of memory size used during execution

 - `sap.hana.statement.active.execution.sum` (*gauge*)<br>    Sum of statement execution time

 - ***`sap.hana.statement.active.execution.time.max`*** (*gauge*)<br>    Maximum time of statement execution

 - ***`sap.hana.statement.active.execution.time.mean`*** (*gauge*)<br>    Average time of statement execution

 - ***`sap.hana.statement.expensive.count`*** (*counter*)<br>    Number of occurances of the statement

 - ***`sap.hana.statement.expensive.cpu_time`*** (*counter*)<br>    CPU time consumed to compute the statement in microseconds

 - ***`sap.hana.statement.expensive.duration`*** (*counter*)<br>    Time elapsed during execution of the statement in microseconds

 - ***`sap.hana.statement.expensive.errors`*** (*counter*)<br>    Number of errors

 - ***`sap.hana.statement.expensive.lock_wait_duration`*** (*counter*)<br>    Accumulated lock wait duration

 - ***`sap.hana.statement.expensive.records`*** (*counter*)<br>    Number of records

 - ***`sap.hana.table.record.count`*** (*gauge*)<br>    Number of records in the table

 - ***`sap.hana.table.size`*** (*gauge*)<br>    Allocated memory size for fixed-size and variable-size part


### Non-default metrics (version 4.7.0+)

To emit metrics that are not _default_, you can add those metrics in the
generic monitor-level `extraMetrics` config option.  Metrics that are derived
from specific configuration options that do not appear in the above list of
metrics do not need to be added to `extraMetrics`.

To see a list of metrics that will be emitted, you can run `agent-status
monitors` after configuring this monitor in a running agent instance.

## Dimensions

The following dimensions might occur on metrics emitted by this monitor.  Some
dimensions might be specific to certain metrics.

| Name | Description |
| ---  | ---         |
| `app_user` | Application user name |
| `component_name` | Logical component |
| `connection_status` | Connection status (RUNNING, IDLE, QUEUING) |
| `db_user` | User name |
| `hana_host` | SAP HANA host name |
| `operation` | Type of operation (prepare, execute, fetch, or close) |
| `schema_name` | Schema name |
| `service_name` | Service name |
| `statement_hash` | Unique identifier for an SQL string. |
| `table_name` | Table name |
| `table_type` | Type of the table (ROW, COLUMN, COLLECTION) |
| `type` | Type of contained files |
| `usage_type` | Resource type like LOG, DATA, TRACE, LOG_BACKUP and DATA_BACKUP |



## Troubleshooting

```{include} /_includes/troubleshooting.md
```