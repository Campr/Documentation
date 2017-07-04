# Server Information \(server\_info\)

This endpoint allows application to know more about the server they're currently interacting with. Everything here is contextual to the requesting application, and may not reflect what the user is seeing when administrating their server.

{% method %}
## GET/HEAD - Retrieve server information

#### Response

| Property    | Type                                                 | Description                                        |
| ----------- | ---------------------------------------------------- | -------------------------------------------------- |
| `global`    | UserUse                                              | Global view of quotas and use for the user.        |
| `types`     | Map&lt;String, TypeUse&gt;                           | Quotas and use by post type.                       |
| `endpoints` | Map&lt;String, Map&lt;String, EndpointLimits&gt;&gt; | Limits and rate-limits by endpoint+method.         |

{% sample lang="http" %}
#### Example request

```
GET /server-info
```

#### Example response

```
HTTP/1.1 200 OK
Content-Type: application/json
```

```json
{
  "global": {
    "space_used": 543896,
    "space_available": 150000000
  },
  "types": {
    "https://campr.me/types/status/v1": {
      "count": 123,
      "space_used": 253089,
      "space_available": 150000000
    }
  },
  "endpoints": {
    "posts": {
      "GET": {
        "list_limit_default": 50,
        "list_limit_maximum": 200,
        "rate_limit_limit": 100,
        "rate_limit_remaining": 54,
        "rate_limit_reset": 12
      },
      "POST": {
        "rate_limit_limit": 50,
        "rate_limit_remaining": 49,
        "rate_limit_reset": 12
      }
    },
    ...
    "batch": {
      "POST": {
        "requests_limit": 50,
        "rate_limit_limit": 50,
        "rate_limit_remaining": 42,
        "rate_limit_reset": 12
      }
    },
    ...
  }
}
```
{% endmethod %}