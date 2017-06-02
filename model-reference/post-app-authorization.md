# Post: App Authorization

A grant of permissions to a specific client for a given app. This allows for multiple clients to use the same app in parallel, and for them to be individually revoked if necessary.

The permissions on the Authorization {{ protocol-post }} can differ from the ones specified on the app. It's validity can also be limited in time.

{% extends "../templates/model.md" %}
{% block properties %}
| | `app_id`     | Integer      | ID of the App to which the Authorization have been granted
| | `token`      | String       | Token to be used in the `Authorization` header
| | `scopes`     | Array[Scope] | Array of the Scopes granted by the client
|Ø| `expires_at` | Timestamp    | Date until which the App Authorization is valid
{% endblock %}


{% block json %}
```json
{
    "app_id": "",
    "expires_at": 123,
    "token": "",
    "scopes": ["*"]
}
```
{% endblock %}
