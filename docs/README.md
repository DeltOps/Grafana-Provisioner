# Docs

# Configure the Provider
In order to connect to your instance, you can either:
- Provide grafana credentials in the config file
    ```
    grafana:
      url: https://grafana.example.com
      auth: username:password
    ```

- Use environment variable
    ```
    GRAFANA_AUTH="username:password" \
    GRAFANA_URL="https://grafana.example.com" \
    terraform apply
    ```
    A list of all possible variables are defined in [the terraform doc](https://registry.terraform.io/providers/grafana/grafana/latest/docs#schema).


## Configurable Resources
- [Alerts](alerts/README.md)
- [Contacts](contacts/README.md)
- [Dashboards](dashboards/README.md)
- [Datasources](datasources/README.md)
- [Folders](folders/README.md)

## Terraform references
When dealing with the code, or a template, the Terraform Resources can be referenced using the name defined in the config file using:
```
grafana_rule_group.{{ name | lower | replace('-', '_') }}
grafana_contact_point.{{ name | lower | replace('-', '_') }}
grafana_dashboard.{{ name | lower | replace('-', '_') }}
grafana_data_source.{{ name | lower | replace('-', '_') }}
grafana_folder.{{ name | lower | replace(' -', '_') }}
```

