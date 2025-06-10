# Rules Subsection
A rule is part of an rule group. See [rule group doc](README.md) to get started.

The only tested datasource is Prometheus. Feel free to contribute to add more datasources !

## Parameters
| Parameter | Description |
| --- | --- |
| (required) `name` | The name of the rule group. Should be unique. |
| (required) `expr` | The query to run on the datasource. |
| (required) `value` | The threshold to compare to the result of the query. Should be a floating point number (eg: `1.0` instead of `1`). |
| (required) `condition` | The comparing logic to apply between `expr` and `value`. Can be `lt` (less than) or `gt` (greater than) |
| `vector` | Whether the query should returns a `range` or an `instant` vector. Range is used for visualizing the alert graph in the web interface, but the datasource can timeout for high-cardinality metrics. (Default: `instant`)|

## Examples
```
...
    rules:
      - name: High CPU Load
        expr: |
          max(
            node_load1 / count(node_cpu_seconds_total{mode="idle"})
          ) by (instance)
        condition: gt
        value: 0.9
        vector: range
      - name: High Memory Usage
        expr: |
          max(
            1 - node_memory_MemAvailable_bytes / node_memory_MemTotal_bytes
          ) by (instance)
        condition: gt
        value: 0.8
        vector: range
```
