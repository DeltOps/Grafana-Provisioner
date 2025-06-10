# Alerts Section
This allows you to manage only Grafana Managed Alerting Rules. Feel free to contribute to add more alerting rules sources.

Among the little things to know:
- alerts rules are grouped into `grafana_rule_group` ([see the doc](https://registry.terraform.io/providers/grafana/grafana/latest/docs/resources/rule_group))
- every group need to be attached to a `grafana_folder` ([see the doc](https://registry.terraform.io/providers/grafana/grafana/latest/docs/resources/folder)).

This page documents only the rule group specifications, see [RULES.md](RULES.md) for details about the rules themselves.

Starting writing alerts assumes you already have configured:
- one folder ([see doc](../folders/README.md)
- one datasource ([see doc](../datasources/README.md)
- one contact ([see doc](../contacts/README.md)

## Parameters
| Parameter | Description |
| --- | --- |
| (required) `name` | The name of the rule group. Should be unique. |
| (required) `interval_seconds` | The number of seconds between each evaluation of the rules in the group. |
| (required) `for` | The amount of time for which the rule must be breached for the rule to be considered to be Firing. Before this time has elapsed, the rule is only considered to be Pending. Available suffixes: `s` (seconds, default), `m` (minutes), `h` (hours). |
| (required) `folder` | The folder to attach the rule group to, should be defined in the config file. |
| (required) `datasource` | The datasource's name, should be defined in the config file. |
| (required) `contact` | The contact's name, should be defined in the config file. |
| (required) `rules` | A list of rules. See [RULES.md](RULES.md). |
| `orgId` | The ID of the Grafana Organization (default: `1`). |
| `noDataState` | Describes what state to enter when the rule's query returns No Data. Options are OK, NoData, KeepLast, and Alerting (default: "NoData"). |

## Example
Create an `alert_group` called `node_exporter_capacity` attached to `Alerts` folder. The group gets evaluated every `60` seconds and would fire if the rule written in the `Prometheus`' datasource language is evaluated to true for more than `15 minutes`, and would alert the Default contact.
```
alerts:
  - name: node_exporter_capacity
    interval_seconds: 60
    for: 15m
    folder: Alerts
    datasource: Prometheus
    contact: Default
    rules:
      - name: High CPU Load
        expr: |
          max(
            node_load1 / count(node_cpu_seconds_total{mode="idle"})
          ) by (instance)
        condition: gt
        value: 0.9
        vector: range

# Dependencies
datasources:
  - name: Prometheus
    url: http://prometheus-server:9090
    type: prometheus

folders:
  - name: Alerts

contacts:
  - name: Default
    type: slack
    slack:
      url: <MATTERMOST_WEBHOOK>
      username: Grafana Alerts
      mention_channel: "here"
```

## See also
- [Terraform Resource](https://registry.terraform.io/providers/grafana/grafana/latest/docs/resources/rule_group)
- [Grafana's Official Documentation](https://grafana.com/docs/grafana/latest/alerting/) (not really useful so far, but maybe some day)
