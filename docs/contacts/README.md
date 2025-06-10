# Contacts Section
A contact is a way to receive alerts from Grafana. The official grafana doc for terraform refers to it as `grafana_contact_point` ([see doc](https://registry.terraform.io/providers/grafana/grafana/latest/docs/resources/contact_point))

There are multiple ways to get alerts from Grafana. Currently supported/tested methods are:
- Slack/Mattermost WebHook URL

The full list is available [here](https://registry.terraform.io/providers/grafana/grafana/latest/docs/resources/contact_point#optional) but you might need a little bit of tweak in order to get it working. Contributions are welcome !

## Parameters
| Parameter | Description |
| --- | --- |
| (required) `name` | The name of the contact point. Should be unique. |
| (required) `type` | the type of contact point. Can be `slack` for now, but you can implement others. |
| `disable_resolve_message` | Whether to send "resolved alerts" messages (default: `false`). |
| `slack` | If the type is slack. See below for specific parameters. |

### Slack Parameters
Should be included in the `slack` section of the `type: slack` contact.
| Parameter | Description |
| --- | --- |
| (required) `url` | The URL to the WebHook |
| `username` | Username to show in the messages. |
| `mention_channel` | If set, the channel will be mentionned. Possible values are `here` or `channel` (default: `""`). |
| `mention_groups` | The list of groups to mention (default: `[]`). |
| `mention_users` | The list of users to mention (default: `[]`). |

```
contacts:
  - name: Default
    type: slack
    disable_resolve_message: false
    slack:
      url: <MATTERMOST_WEBHOOK_URL>
      username: Grafana Alerts
      mention_channel: here
      mention_users:
        - stephan
        - georgio
      mention_groups:
        - marketing
        - sales
```

## See also
[Terraform grafana_contact_point](https://registry.terraform.io/providers/grafana/grafana/latest/docs/resources/contact_point)
