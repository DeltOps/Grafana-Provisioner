# Folder Section
A folder is used to hold dashboards, alerts, and probably more. Only top-level folders are supported for now. Feel free to contribute!

## Parameters
| Parameter | Description |
| --- | --- |
| (required) `name` | The name of the datasource. Should be unique. |

## Example
```
folders:
- name: Dashboards
- name: Alerts
```

## See Also
- [Terraform doc for grafana_folder](https://registry.terraform.io/providers/grafana/grafana/latest/docs/resources/folder)
