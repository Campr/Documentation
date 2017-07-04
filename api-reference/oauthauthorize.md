# Initiate OAuth flow \(oauth\_authorize\)

The Protocol authentication flow takes inspiration from the [OAuth 2.0 Code Grant](https://tools.ietf.org/html/rfc6749#section-4.1) flow, with a few differences.

{% method %}
## GET - Initiate authentication flow

The user must be directed to this endpoint to initiate the OAuth authentication flow.

Unlike traditional OAuth, the `redirect_uri` and `scope` parameters are not supported as their value will be specified during [App registration](/app-layer). They should be ignored if included.

If a `response_type` parameter is provider, its value needs to be `code`. Any other value will result in an `unsupported_response_type` error.

After the user authorizes the app, they are redirected back with the `code` necessary for the [next step](/api-reference/oauthtoken) of the authentication flow.

#### Request parameters

| | Parameter   | Type   | Description                                            |
|-| ----------- | ------ | ------------------------------------------------------ |
| | `client_id` | String | The post ID of the app making this request.            |
|Ø| `state`     | String | An arbitratry state for the app to track the response. |

#### Successful redirection parameters

| | Parameter | Type   | Description                                                                          |
|-| --------- | ------ | ------------------------------------------------------------------------------------ |
| | `code`    | String | The short-lived code to [exchange](/api-reference/oauthtoken) for an `access_token`. |
| | `state`   | String | The arbitrary state provided during the request.                                     |

#### Error redirection parameters

| | Parameter           | Type   | Description                                                          |
|-| ------------------- | ------ | -------------------------------------------------------------------- |
| | `state`             | String | The arbitrary state provided during the request.                     |
| | `error`             | Enum   | If the authentication succeeded, a code will be provided.            |
|Ø| `error_description` | String | Human-readable information about the error.                          |
|Ø| `error_uri`         | String | A URI to a human-readable web-page with information about the error. |

#### Error codes

The following error codes can be returned if the operation does not succeed:

| Code                        | Description                                                                      |
| --------------------------- | -------------------------------------------------------------------------------- |
| `invalid_request`           | The request and/or its parameters are invalid.                                   |
| `unauthorized_client`       | The requesting client is not a known app.                                        |
| `access_denied`             | The requesting client has been blocked.                                          |
| `unsupported_response_type` | The `response_type` parameter was specified with a value different from `code`.  |
| `server_error`              | The authorization server ecountered an unexpected error.                         |
| `temporarily_unavailable`   | The authorization server is temporarily unable to handle authorization requests. |


{% sample lang="http" %}
#### Example request

```
GET /oauth_authorize?client_id=b3f84daaec10476bad8025974e1696cc&state=K57aCn7L9Z
```

#### Example successful response

```
HTTP/1.1 302 Found
Location: https://app.example.com/oauth?code=xA9XjJ3htG9o9DjNIswr&state=K57aCn7L9Z
```

#### Example error response

```
HTTP/1.1 302 Found
Location: https://app.example.com/oauth?error=temporarily_unavailable&state=K57aCn7L9Z
```

{% endmethod %}