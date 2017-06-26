# Multiple actions in a single request \(batch\)

To avoid unecessary round-trips between the client and server, apps are encouraged to make extensive use of this endpoint. It allows multiple requests to be processed in just one HTTP request, and can greatly improve application performance.

{% method %}
## POST - Multiple requests in one call

This is a multipart request where every part is its own HTTP request. The response is also multipart, with matching IDs to match requests with responses.

`GET` requests will be processed in parallel.
`POST`, `PUT`, `PATCH`, and `DELETE` requests will run in the order in which they're specified.

The following endpoints can be queried as part of a batch request: `posts`, `post`, `attachments`, `attachment`.

{% sample lang="http" %}
#### Example request

```
TODO.
```

#### Example response

```
TODO.
```
{% endmethod %}