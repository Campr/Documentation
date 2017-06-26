# Post Collection \(posts\)

This endpoint allows interactions with the post collection: to create new posts and to list existing ones.

{% method %}
## GET/HEAD - List Posts from the collection



Related resources, linked posts and user profiles, can be returned along with the Posts if requested.

#### Parameters

| Parameter  | Type                          | Default | Description                                                 |
| ---------- | ----------------------------- | ------- | ----------------------------------------------------------- |
| `limit`    | Integer                                                                           | 50 | The maximum number of items to return. |
| `sort_by`  | Enum `received_at`, `published_at`, `version.received_at`, `version.published_at` | received_at | The sort order. |
| `after`    | Datetime+PostVersion
| `before`   | Datetime+PostVersion
| `types`    | Condition&lt;PostType&gt;
| `entities` | Condition&lt;Entity&gt;
| `links`    | Condition&lt;Post&gt;
| `related`  | MultiEnum `links`, `profiles` | Nothing | The resources related to this post that should be returned. |

#### Response

| | Property   | Type                                                                   | Description                             |
|-| ---------- | ---------------------------------------------------------------------- | --------------------------------------- |
| | `posts`    | Array&lt;[{{ book.protocolPost }}](/model-reference/post-envelope)&gt; | The requested {{ book.protocolPosts }}.  |
|Ø| `links`    | Array&lt;[{{ book.protocolPost }}](/model-reference/post-envelope)&gt; | Linked {{ book.protocolPosts }}.        |
|Ø| `profiles` | Map&lt;Profile&gt;                                                   | Profiles for the user and linked users. |

{% sample lang="http" %}
#### Example request

```
GET /posts?related=links,profiles
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
## POST - Create a new post

Add a new post to the collection.

This post can reference zero or more parents.
It can be private (default), public, or shared with specific entities.

Attachments should reference the SHA3-256 digest from a [detached file](/api-reference/attachments), or from another post in this user's [post collection](/api-reference/posts#post-collection).

This request will fail if any of the links or attachments failed to be resolved.

#### Parameters

| Parameter | Type                          | Default | Description                                                 |
| --------- | ----------------------------- | ------- | ----------------------------------------------------------- |
| `related` | MultiEnum `links`, `profiles` | Nothing | The resources related to this post that should be returned. |

#### Request

| | Property          | Type                 | Description                                      |
|-| ----------------- | -------------------- | ------------------------------------------------ |
| | `content`         | Object               | The content of the post.                         |
| | `type`            | Post type URL        | The type of the post.                            |
|Ø| `permissions`     | Permissions          | Permissions for the post. Defaults to private.   |
|Ø| `version.message` | String               | A comment associated with this specific version. |
|Ø| `version.parents` | Array<PostReference> | The other posts/versions this one was based on.  |

#### Response

| | Property   | Type                                                                   | Description                             |
|-| ---------- | ---------------------------------------------------------------------- | --------------------------------------- |
| | `post`     | [{{ book.protocolPost }}](/model-reference/post-envelope)              | The new {{ book.protocolPost }}.        |
|Ø| `links`    | Array&lt;[{{ book.protocolPost }}](/model-reference/post-envelope)&gt; | Linked {{ book.protocolPosts }}.        |
|Ø| `profiles` | Map&lt;Profile&gt;                                                   | Profiles for the user and linked users. |

{% sample lang="http" %}
#### Example request

```
POST /posts?related=links,profiles
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
## POST (Multipart) - Create a new post with attachments

Add a new post to the collection.

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