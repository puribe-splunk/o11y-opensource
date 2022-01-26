(elasticsearch-query)=

# Elasticsearch query

<meta name="Description" content="Documentation on the elasticsearch-query monitor">

## Description

**This receiver is in beta.**

This receiver metricizes aggregated responses from Elasticsearch. The receiver constructs Splunk Observability Cloud data points based on Elasticsearch aggregation types and also aggregation names.

The following is a sample configuration: 

```yaml
monitors:
- type: elasticsearch-query
 host: localhost
 port: 9200
 index: <name_of_index>
 elasticsearchRequest: |
    {
      "query" : {
        "range" : {
          "@timestamp" : {
            "gte": "now-5m"
          }
        }
      },
      "aggs": {
        "avg_cpu_utilization": {
          "avg": {
            "field": "cpu_utilization"
          }
        }
      }
    }
intervalSeconds: 300
```

The `elasticsearchRequest` takes in a `string` request in the format specified in 
[elasticsearch documentation](https://www.elastic.co/guide/en/elasticsearch/reference/current/search-request-body.html).

The above query is performed against an index containing documents that take the following form:

```
{
   'cpu_utilization':87,
   'memory_utilization':94,
   'host':'helsinki',
   'service':'android',
   'container_id':'macbook',
   '@timestamp':1580321240579
}
```

The query specified in `elasticsearchRequest` returns the average value of `cpu_utilization` across all documents with a `@timestamp`
in the last five minutes. This value is metricized to the following form in Splunk Observability Cloud:

```
{
metric_name: avg_cpu_utilization,
dimensions:
  index: <name_of_index>
  metric_aggregation_type: avg
}
```

## Data model transformation

This receiver transforms Elasticsearch responses into Splunk Observability Cloud data points.

At high level, this receiver metricizes responses of the following types:

1. Metric aggregations inside one or more Bucket aggregations such as the `terms` and `filters` aggregations. Dimensions on a data point are determined by the aggregation name (dimension name) and the `key` of each bucket (dimension value). The metric name is derived from the type of metric aggregation name and its values in case of multi-value aggregations. A dimension called `metric_aggregation_type`is also set on the corresponding data points. 

2. Metric aggregations applied without any Bucket aggregation are transformed in the same way.

3. Bucket aggregations that do not have any Metric aggregations as sub aggregations are
transformed to a metric called `<name_of_aggregation>.doc_count` and have the `bucket_aggregation_name`
dimension apart from the `key` of each bucket.

**Note**: Since Bucket aggregations determine dimensions in Splunk Observability Cloud, in most cases Bucket aggregations should be performed on `string` fields that represent a slice of the data from Elasticsearch.

## Examples

1. `avg` metric aggregation as a sub-aggregation of `terms` bucket aggregation: 


    ```json
    {
    "aggs":{
        "host" : {
        "terms":{"field" : "host"},
        "aggs": {
            "average_cpu_usage": {
            "avg": {
                "field": "cpu_utilization"
            }
            }
        }
        }
    }
    }
    ```

    This query results in a metric called `elasticsearch_query.average_cpu_usage`, where the data point has a `host` dimension with its value being the `key` of a bucket in the response. The type of the metric aggregation (`avg`) is set on the data oint as the `metric_aggregation_type` dimension. 

    For instance, the `json` below provides 4 data points, each with a different value for `host`:

    ```json
    "aggregations" : {
    "host" : {
        "doc_count_error_upper_bound" : 0,
        "sum_other_doc_count" : 0,
        "buckets" : [
        {
            "key" : "helsinki",
            "doc_count" : 13802,
            "average_cpu_usage" : {
            "value" : 49.77438052456166
            }
        },
        {
            "key" : "lisbon",
            "doc_count" : 13802,
            "average_cpu_usage" : {
            "value" : 49.919866685987536
            }
        },
        {
            "key" : "madrid",
            "doc_count" : 13802,
            "average_cpu_usage" : {
            "value" : 49.878350963628456
            }
        },
        {
            "key" : "nairobi",
            "doc_count" : 13802,
            "average_cpu_usage" : {
            "value" : 49.99789885523837
            }
        }
        ]
    }
    }
    ```

2. `extended_stats` metric aggregation as a sub-aggregation of `terms` bucket aggregation


    ```json
    {
    "aggs":{
    "host" : {
        "terms":{"field" : "host"},
        "aggs": {
        "cpu_usage_stats": {
            "extended_stats": {
            "field": "cpu_utilization"
            }
        }
        }
    }
    }
    }
    ```

    ```json
    "aggregations" : {
    "host" : {
        "doc_count_error_upper_bound" : 0,
        "sum_other_doc_count" : 0,
        "buckets" : [
        {
            "key" : "helsinki",
            "doc_count" : 13996,
            "cpu_usage_stats" : {
            "count" : 13996,
            "min" : 0.0,
            "max" : 100.0,
            "avg" : 49.86660474421263,
            "sum" : 697933.0
            }
        },
        {
            "key" : "lisbon",
            "doc_count" : 13996,
            "cpu_usage_stats" : {
            "count" : 13996,
            "min" : 0.0,
            "max" : 100.0,
            "avg" : 49.88225207202058,
            "sum" : 698152.0
            }
        },
        {
            "key" : "madrid",
            "doc_count" : 13996,
            "cpu_usage_stats" : {
            "count" : 13996,
            "min" : 0.0,
            "max" : 100.0,
            "avg" : 49.92469276936267,
            "sum" : 698746.0
            }
        },
        {
            "key" : "nairobi",
            "doc_count" : 13996,
            "cpu_usage_stats" : {
            "count" : 13996,
            "min" : 0.0,
            "max" : 100.0,
            "avg" : 49.98320948842527,
            "sum" : 699565.0
            }
        }
        ]
    }
    }
    ```

    In this case, each bucket results in 5 metrics:

    1. `cpu_usage_stats.count`
    2. `cpu_usage_stats.min`
    3. `cpu_usage_stats.max`
    4. `cpu_usage_stats.avg`
    5. `cpu_usage_stats.sum`

    The dimensions are derived in the same manner as the previous example.

### Benefits

```{include} /_includes/benefits.md
```

## Installation

```{include} /_includes/collector-installation.md
```

## Configuration

```{include} /_includes/configuration.md
```

```
receivers:
  smartagent/elasticsearch-query: 
    type: elasticsearch-query
    ... # Additional config
```

To complete the integration, include the receiver with this monitor type in a `metrics` pipeline. To do this, add the receiver to the `service > pipelines > metrics > receivers` section of your configuration file.

```
service:
  pipelines:
    metrics:
      receivers: [smartagent/elasticsearch-query]
```

See the [configuration example](https://github.com/signalfx/splunk-otel-collector/tree/main/examples) in GitHub for specific use cases that show how the Splunk OpenTelemetry Collector can integrate and complement existing environments.

For Kubernetes, see the [kubernetes.yaml](https://github.com/signalfx/splunk-otel-collector/tree/main/examples/kubernetes-yaml) in GitHub for the Agent and Gateway YAML files.

For Prometheus, see [Prometheus Federation Endpoint Example](https://github.com/signalfx/splunk-otel-collector/tree/main/examples/prometheus-federation) in GitHub for an example of how the OTel Collector works with Splunk Enterprise and an existing Prometheus deployment.

### Configuration settings

The following table shows the configuration options for this receiver:

| Option | Required | Type | Description |
| --- | --- | --- | --- |
| `httpTimeout` | no | `int64` | HTTP timeout duration for both read and writes. This should be a duration string that is accepted by https://golang.org/pkg/time/#ParseDuration (**default:** `10s`) |
| `username` | no | `string` | Basic Auth username to use on each request, if any. |
| `password` | no | `string` | Basic Auth password to use on each request, if any. |
| `useHTTPS` | no | `bool` | If true, the agent will connect to the server using HTTPS instead of plain HTTP. (**default:** `false`) |
| `httpHeaders` | no | `map of strings` | A map of HTTP header names to values. Comma separated multiple values for the same message-header is supported. |
| `skipVerify` | no | `bool` | If useHTTPS is true and this option is also true, the exporter's TLS cert will not be verified. (**default:** `false`) |
| `caCertPath` | no | `string` | Path to the CA cert that has signed the TLS cert, unnecessary if `skipVerify` is set to false. |
| `clientCertPath` | no | `string` | Path to the client TLS cert to use for TLS required connections |
| `clientKeyPath` | no | `string` | Path to the client TLS key to use for TLS required connections |
| `host` | **yes** | `string` |  |
| `port` | **yes** | `string` |  |
| `index` | no | `string` | Index that's being queried. If none is provided, given query will be applied across all indexes. To apply the search query to multiple indices, provide a comma separated list of indices (**default:** `_all`) |
| `elasticsearchRequest` | **yes** | `string` | Takes in an Elasticsearch request body search request. See the Elasticsearch documentation at https://www.elastic.co/guide/en/elasticsearch/reference/current/search-request-body.html for details. |

## Metrics

The Splunk Distribution of OpenTelemetry Collector does not do any built-in filtering of metrics for this receiver.

## Troubleshooting

```{include} /_includes/troubleshooting.md
```
