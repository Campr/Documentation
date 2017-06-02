# Single Post \(post\)

The `post` endpoint template has the following parameters:

| | Key       | Description          |
|-| --------- | -------------------- |
| | `post`    | The ID of the post   |
|Ø| `version` | The version the post |

{% method %}
## Retrieve a single {{ book.protocolPost }}

Retrieve a specific  [{{ book.protocolPost }}](/model-reference/post-envelope).
If no version is specified, the latest is returned.

Related resources, linked posts and user profiles, can be returned along with the {{ book.protocolPost }} if requested.

#### Definition

```
GET post(post, version?)
Accept: application/vnd.protocol.post.v1+json
```

#### Parameters

| Parameter | Type                          | Default | Description                                                 |
| --------- | ----------------------------- | ------- | ----------------------------------------------------------- |
| `related` | MultiEnum `links`, `profiles` | Nothing | The resources related to this post that should be returned. |

#### Response

| | Property   | Type                                                                   | Description                             |
|-| ---------- | ---------------------------------------------------------------------- | --------------------------------------- |
| | `post`     | [{{ book.protocolPost }}](/model-reference/post-envelope)              | The requested {{ book.protocolPost }}.  |
|Ø| `links`    | Array&lt;[{{ book.protocolPost }}](/model-reference/post-envelope)&gt; | Linked {{ book.protocolPosts }}.        |
|Ø| `profiles` | Array&lt;Profile&gt;                                                   | Profiles for the user and linked users. |

{% sample lang="http" %}
#### Example request

```
GET /posts/b3f84daaec10476bad8025974e1696cc HTTP/1.1
Accept: application/vnd.protocol.post.v1+json
```

#### Example response

```
HTTP/1.1 200 OK
Content-Type: application/vnd.protocol.post.v1+json; type="https://campr.me/types/status/v1"
```

```json
{
  "post": {
    ...
  },
  "links": [{
    ...
  }],
  "profiles": {
    "https://user1.{{ book.providerDomain }}": {
      ...
    }
  }
}
```
{% endmethod %}