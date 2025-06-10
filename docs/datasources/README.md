# Section Datasources
Datasources implementation is incomplete. Feel free to contribute!

## Parameters
| Parameter | Description |
| --- | --- |
| (required) `name` | The name of the datasource. Should be unique. |
| (required) `type` | The datasource type. |
| (required) `url` | The datasource's URL |

## Example
```
datasources:
  - name: Prometheus
    url: http://prometheus-server:9090
    type: prometheus
```

## See Also
- [Terraform doc for grafana_data_source](https://registry.terraform.io/providers/grafana/grafana/latest/docs/resources/data_source)
