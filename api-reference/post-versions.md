# Post versions \(post_versions\)

The `post_versions` endpoint template has the following parameters:

| | Key       | Description             |
|-| --------- | ----------------------- |
| | `post`    | The ID of the post.     |

{% method %}
## GET/HEAD - List the versions of a {{ book.protocolPost }}

Retrieve a list of all the versions for the specified post.
A `HEAD` request can also be performed if only the count is needed.

#### Response

| | Property   | Type                 | Description                         |
|-| ---------- | -------------------- | ----------------------------------- |
| | `versions` | Array&lt;Version&gt; | The versions of the specified post. |

{% sample lang="http" %}
#### Example request

```
GET /post_versions/b3f84daaec10476bad8025974e1696cc
```

#### Example response

```
HTTP/1.1 200 OK
Count: 10
```

```json
{
  "versions": [{
    ...
  }]
}
```
{% endmethod %}