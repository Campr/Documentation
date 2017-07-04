# OAuth Access Token \(oauth\_token\)

After a short-lived code is obtained during the [authorize](/api-reference/oauth-authorize) process, the client can now exchange it for permanent user credentials.

All requests to this endpoint need to be [authenticated](/api-reference/authentication) with [app credentials](/app-layer).

{% method %}
## POST - Exchange code for user credentials

Exchange a temporary `code` for permanent user credentials. They can then be used to perform requests on behalf of the user for the scopes requested during [app registration](/app-layer).

The only token type current supported is `https://campr.me/oauth/hawk-token`. More could be introduced in the future to support different use-cases.

#### Request

| Property     | Type   | Description                                |
| ------------ | ------ | ------------------------------------------ |
| `code`       | String | The code to exchange for user credentials. |
| `token_type` | String | The type of token to exchange it for.      |

#### Response

| Property     | Type                                                      | Description                |
| ------------ | --------------------------------------------------------- | -------------------------- |
| `post`       | [{{ book.protocolPost }}](/model-reference/post-envelope) | The user credentials post. |

{% sample lang="http" %}
#### Example request

```
POST /oauth_token
```

```json
{
  "code": "xA9XjJ3htG9o9DjNIswr",
  "token_type": "https://campr.me/oauth/hawk-token"
}
```

#### Example response

```
HTTP/1.1 200 OK
Content-Type: application/json; type="https://campr.me/types/credentials/v1"
```

```json
{
  "post": {
    "id": "8RyhNzzHZvYG7P4wJbG1",
    "type": "https://campr.me/types/credentials/v1",
    "content": {
      "hawk_key": "Vofn1nS99gg5WoJeXY4S",
      "hawk_algoritm": "sha3-256"
    }
  }
}
```

{% endmethod %}

