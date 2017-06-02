# Single Post \(post\)

| | Key     | Description          |
|-| ------- | -------------------- |
| | post    | The ID of the post   |
|Ø| version | The version the post |

{% method %}
## Retrieve a single {{ book.protocolpost }}

Retrieve a specific  [{{ book.protocolpost }}](/model-reference/post-envelope).
If no version is specified, the latest is returned.

Related resources, linked posts and user profiles, can be returned along with the {{ book.protocolpost }} if requested.

{% sample lang="http" %}
#### Definition

```
GET post(post, version?) HTTP/1.1
Accept: application/vnd.{{ book.protocolnamelower }}.{{ book.protocolpostlower }}.v1+json
```

#### Parameters

| Parameter | Type                       | Default | Description                                                 |
| --------- | -------------------------- | ------- | ----------------------------------------------------------- |
| `related` | MultiEnum (links|profiles) | Nothing | The resources related to this post that should be returned. |

#### Response

| | Property   | Type                                                             | Description                             |
|-| ---------- | ---------------------------------------------------------------- | --------------------------------------- |
| | `post`     | [{{ book.protocolpost }}](/model-reference/post-envelope)        | The requested {{ book.protocolpost }}.  |
|Ø| `links`    | Array<[{{ book.protocolpost }}](/model-reference/post-envelope)> | Linked {{ book.protocolPosts }}.        |
|Ø| `profiles` | Array<Profile>                                                   | Profiles for the user and linked users. |

#### Example response

```
HTTP/1.1 200 OK
Content-Type: application/vnd.{{ book.protocolnamelower }}.{{ book.protocolpostlower }}; type="..."
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
    "{entity}": {
      ...
    }
  }
}
```
{% endmethod %}