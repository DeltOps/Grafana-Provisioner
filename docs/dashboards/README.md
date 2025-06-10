# Dashboards Section
Dashboards can be provisioned by Terraform. However they can still be modified in the UI. See [issue #2187](https://github.com/grafana/terraform-provider-grafana/issues/2187).
Provisioned dashboards are automatically attached to the default datasource, or the dashboard's json needs to be templated. See below for templated dashboards.

## Parameters
| Parameter | Description |
| --- | --- |
| (required) `name` | The name of the dashboard. Should be unique. |
| `mode` | `local` or `http`. See below for more info. (Default: `local`) |
| `folder` | The folder to put the dashboard in. The folder must be defined in the config file. (default: "") |
| `url` | Required when `mode` is `http`. URL to download the dashboard from. |
| `template` | A dict of templating variables, used only for local dashboards. See below for more info. |

## Local dashboards
Local dashboards are already present locally under `dashboards/<name>.json`. The dashboard's `name` in the config should match the file's name, minus the `.json` extension.
```
- name: test # Read file "dashboards/test.json"
  mode: local
```

## HTTP dashboards
Since Grafana has a lot of great community dashboards, you can download them using the `mode: http`, but you need to add the `url` to download the dashboard.
```
- name: node-exporter
  mode: http
  url: "https://grafana.com/api/dashboards/1860/revisions/40/download"
```
Downloaded dashboards are also stored in `dashboards/<name>.json`. You can comment the `mode: http` line if you want to use the already downloaded dashboard and avoid downloading it again.

## Templated dashboards (advanced)
Refactor of the dashboard templating mecanism is welcome !

Some dashboards need to be templated. For exemple, a dashboard displaying firing alerts. Since alerts are attached to a folder, a dashboard displaying alerts needs to know which folder to query for alerts. There is no native way of doing it (so far). There is also a problem about attaching a dashboard to a datasource that is not the default one. This is a bit painful to adapt but still possible with templating.

Templated dashboards are only `mode: local` dashboards. And the extension for the file is not `.json` but `.json.j2`. This allows to easily detect if a dashboard file is a template or not.

Templated dashboards are templated twice:
- First by jinja2. You can access variables defined in `template` section of your dashboard's configuration using the `dash` dictionnary.
    Eg: `{{ dash.name }}` would print the content of `.dashboards[0].template.name`.
- Second is the Terraform interpreter. Your dashboard will be renderer by terraform, which allows you to reference terraform resources.
    Eg: `${ grafana_data_source.prometheus.uid }` would result in the UID of the datasource named "prometheus".

You might want to read the `templates/` directory content in order to better reference other resources.

There are examples of templated dashboards in `/dashboards/` with `.json.j2` suffix. Check them out for reference.

When exporting dashboards for templating from UI, make sure you don't check the "Export the dashboard to use in another instance". It would define the datasources as variables that need to be set when imported, and the terraform provider has no native way of doing it. See [Issue #1040](https://github.com/grafana/terraform-provider-grafana/issues/1040).

## Issue with Jinja and Grafana Verbose Legends
Since Grafana uses `{{ }}` to references variables in panels' verbose legends, you might want to escape the `legendFormat` lines in your template, like:
```
...
    {% raw %}
    "legendFormat": "{{namespace}} - {{pod}}",
    {% endraw %}
...
```
