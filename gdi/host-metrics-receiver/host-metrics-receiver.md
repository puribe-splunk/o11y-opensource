(host-metrics-receiver)=

# Host metrics receiver

<meta name="Description" content="Documentation on the host metrics receiver">

## Description

A receiver is how data gets into the Collector. Receivers support one or more data sources - traces, metrics, or logs.

The host metrics receiver generates metrics about the host system scraped from various sources. Use this receiver when the Collector is deployed as an agent.

The supported pipeline type for this receiver is `metrics`.

### Benefits

```{include} /_includes/benefits.md
```

## Installation

```{include} /_includes/collector-installation.md
```

## Configuration

The collection interval and the categories of metrics to be scraped can be [configured](#scraper-configurations), as shown in the following example.

```yaml
hostmetrics:
  collection_interval: <duration> # The default is 1m.
  scrapers:
    <scraper1>:
    <scraper2>:
    ...
```

The following table shows the available scrapers:

| Scraper    | Supported OS                                                            | Description                                            |
|------------|-------------------------------------------------------------------------|--------------------------------------------------------|
| cpu        | Not supported on macOS when compiled without Cgo, which is the default. | CPU utilization metrics                                |
| disk       | Not supported on macOS when compiled without Cgo, which is the default. | Disk I/O metrics                                       |
| load       | All                                                                     | CPU load metrics                                       |
| filesystem | All                                                                     | File system utilization metrics                        |
| memory     | All                                                                     | Memory utilization metrics                             |
| network    | All                                                                     | Network interface I/O metrics & TCP connection metrics |
| paging     | All                                                                     | Paging or swap space utilization and I/O metrics       |
| processes  | Linux                                                                   | Process count metrics                                  |
| process    | Linux and Windows                                                       | Per process CPU, memory, and disk I/O metrics          |

### Scraper configurations

Scrapers extract data from endpoints and then send that data to a specified target. See the following sections for scraper configurations.

#### Disk

```yaml
disk:
  <include|exclude>:
    devices: [ <device name>, ... ]
    match_type: <strict|regexp>
```

#### File system

```yaml
filesystem:
  <include_devices|exclude_devices>:
    devices: [ <device name>, ... ]
    match_type: <strict|regexp>
  <include_fs_types|exclude_fs_types>:
    fs_types: [ <filesystem type>, ... ]
    match_type: <strict|regexp>
  <include_mount_points|exclude_mount_points>:
    mount_points: [ <mount point>, ... ]
    match_type: <strict|regexp>
```

#### Network

```yaml
network:
  <include|exclude>:
    interfaces: [ <interface name>, ... ]
    match_type: <strict|regexp>
```

#### Process

```yaml
process:
  disk:
    <include|exclude>:
      names: [ <process name>, ... ]
      match_type: <strict|regexp>
```

### Advanced configurations

#### Filtering

To only gather a subset of metrics from a particular source, use the host metrics receiver with the filter processor.

#### Different frequencies

To scrape some metrics at a different frequency than others, configure multiple host metrics receivers with different `collection_interval` values. For example:

```yaml
receivers:
  hostmetrics:
    collection_interval: 30s
    scrapers:
      cpu:
      memory:

  hostmetrics/disk:
    collection_interval: 1m
    scrapers:
      disk:
      filesystem:

service:
  pipelines:
    metrics:
      receivers: [hostmetrics, hostmetrics/disk]

```

## Troubleshooting

```{include} /_includes/troubleshooting.md
```
