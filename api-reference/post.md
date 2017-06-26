# Single Post \(post\)

The `post` endpoint template has the following parameters:

| | Key       | Description                           |
|-| --------- | ------------------------------------- |
| | `entity`  | The entity of the author of the post. |
| | `post`    | The ID of the post.                   |
|Ø| `version` | The version of the post.              |

{% method %}
## GET/HEAD - Retrieve a single {{ book.protocolPost }}

Retrieve a specific [{{ book.protocolPost }}](/model-reference/post-envelope).
If no version is specified, the latest is returned.

Related resources, linked posts and user profiles, can be returned along with the {{ book.protocolPost }} if requested.

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
GET /posts/b3f84daaec10476bad8025974e1696cc?related=links,profiles
```

#### Example response

```
HTTP/1.1 200 OK
Content-Type: application/json; type="https://campr.me/types/status/v1"
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

{% method %}
## PATCH - Update a single {{ book.protocolPost }}

Create a new version of an existing [{{ book.protocolPost }}](/model-reference/post-envelope).
Only a post from our own user can be updated.

At least one of the parents must reference the post being updated.
If a version is provided in the URL template, it will be added as one of the parents for this new version.
If no parent of the same post is provided, the last version will be used.

The type and permissions for this post will be inherited from the first parent unless explicitly specified.

While the `content`, `type`, and `permissions` are all optional, at least one should be provided for a new version to be created.

#### Parameters

| Parameter | Type                          | Default | Description                                                 |
| --------- | ----------------------------- | ------- | ----------------------------------------------------------- |
| `related` | MultiEnum `links`, `profiles` | Nothing | The resources related to this post that should be returned. |

#### Request

| | Property          | Type                 | Description                                      |
|-| ----------------- | -------------------- | ------------------------------------------------ |
|Ø| `content`         | Object               | The new content of the post.                     |
|Ø| `type`            | Post type URL        | The new type of the post.                        |
|Ø| `permissions`     | Permissions          | Updated permissions for the post.                |
|Ø| `version.message` | String               | A comment associated with this specific version. |
|Ø| `version.parents` | Array<PostReference> | The other posts/versions this one was based on.  |

#### Response

| | Property   | Type                                                                   | Description                             |
|-| ---------- | ---------------------------------------------------------------------- | --------------------------------------- |
| | `post`     | [{{ book.protocolPost }}](/model-reference/post-envelope)              | The updated {{ book.protocolPost }}.    |
|Ø| `links`    | Array&lt;[{{ book.protocolPost }}](/model-reference/post-envelope)&gt; | Linked {{ book.protocolPosts }}.        |
|Ø| `profiles` | Array&lt;Profile&gt;                                                   | Profiles for the user and linked users. |

{% sample lang="http" %}
#### Example request

```
PATCH /posts/b3f84daaec10476bad8025974e1696cc?related=links,profiles
```

```json
{
  "type": "https://campr.me/types/status/v1",
  "content": {
    "text": "This is a post"
  }
}
```

#### Example response

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

{% method %}
## DELETE - Delete a single {{ book.protocolPost }}

Delete one of our user's posts. A new version of the post will be created with the type [Deleted](/post-reference/deleted.md). This allows the deletion to be propagated through the network. This new version is returned as a response.

#### Response

| Property | Type                                                      | Description                 |
| -------- | --------------------------------------------------------- | --------------------------- |
| `post`   | [{{ book.protocolPost }}](/model-reference/post-envelope) | The deleted post tombstone. |

{% sample lang="http" %}
#### Example request

```
DELETE /posts/b3f84daaec10476bad8025974e1696cc
```

#### Example response

```json
{
  "post": {
    ...
  }
}
```
{% endmethod %}