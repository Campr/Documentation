# Base Concepts

## Response envelope

All endpoints, at the exception of `attachment` that can return binary data, accept and return data in the JSON format.
Because additional metadata is sometime returned alongside the requested content, all responses to list requests come wrapped in a common envelope.

| Property        | Type              | Description                                      |
| --------------- | ----------------- | ------------------------------------------------ |
| `posts`         | Array<Post>       | A collection of posts.                           |
| `post`          | Post              | A single post.                                   |
| `post_versions` | Array<Version>    | A collection of versions for a specific post.    |
| `attachments`   | Array<Attachment> | A collection of unused attachments for our user. |
| `links`         | Array<Post>       | Linked-to posts.                                 |
| `profiles`      | Array<Profile>    | User profiles.                                   |
| `errors`        | Array<Error>      | A request Error.                                 |

The `links` and `profiles` properties can be controlled through the `related` query parameter. They content is contextual to the main post/posts.

## Pagination

When the requested collection is too large to fit in a single response, pagination is required to retrieve it in multiple requests.
Every list endpoint returns HTTP headers that contain the pagination information necessary to retrieve the requested collection in its entirety.

Every page will be the same size as the first one.

| Header    | Rel                                 | Description                             |
| -------   | ----------------------------------- | --------------------------------------- |
| `Count`   | N/A                                 | Number of posts in the requested range. |
| `Link`    | https://campr.me/rels/first-page    | Link to the first page.                 |
| `Link`    | https://campr.me/rels/previous-page | Link to the previous page.              |
| `Link`    | https://campr.me/rels/next-page     | Link to the next page.                  |
| `Link`    | https://campr.me/rels/last-page     | Link to the last page.                  |

Because they're regular headers, they can be retrieved in a HEAD request if the content is not needed.

## Idempotency

When a request fails, the client finds itself in an inconsistent state, not knowing whether the server actually performed the requested operation or not. But depending on the application, having a request performed twice can have consequences from benign to very serious, making it difficult for the client to systematically retry until it gets a satisfying response.

The solution to this in {{ book.protocolName }} is to specify a `Client-Request-ID` header that will be unique to the request. This ID will be used to deduplicate requests on the server, and any subsequent request with the same content and ID will have its actions performed only once, making it safe for clients to retry again and again. The result will also always be the same.

The following table shows the endpoints and methods that support this mechanism:

| Endpoint   | Method |
| ---------- | ------ |
| `new_post` | POST   |
| `post`     | PATCH  |
| `post`     | DELETE |

## Errors

Errors, if any, will be returned in the `errors` property of the response envelope. They have the following schema:

| | Property      | Type   | Description                                                        |
|-| ------------- | ------ | ------------------------------------------------------------------ |
| | `id`          | String | A unique identifier for this error occurance.                      |
| | `code`        | Enum   | The type of error that occured.                                    |
|Ø| `description` | String | A human-readably description of the Error, for debugging purposes. |

Here's a list of the error codes that can be returned:

| HTTP | Code                | Description                                                           |
| ---- | ------------------- | --------------------------------------------------------------------- |
| 404  | not_found           | The requested post/attachment could not be found.                     |
| 429  | too\_many\_requests | The number of requests for this app is over the rate-limit allowance. |

## Caching

{{ book.protocolName }} servers should be able to handle both the `ETag` and `Last-Modified` cache-invalidation mechanisms, and clients can fully rely on them.

## Rate limiting

To handle real-world usage, server need to be able to rate-limit apps that are not being "good citizens" and use too much resource. While different servers may choose to enforce different rate-limiting policies, some baseline is required to set expectations for app developers. The metrics in the table below constitute the absolute minimum that servers should be able to offer:

| Endpoint          | Method   | Rate (5min window) |
| ----------------- | -------- | ------------------ |
| `posts`           | GET/HEAD | 100                |
| `post`            | GET/HEAD | 300                |
| `post`            | PATCH    | 50                 |
| `post_versions`   | GET/HEAD | 100                |
| `attachments`     | GET/HEAD | 100                |
| `attachment`      | GET/HEAD | 300                |
| `oauth_authorize` | GET      | 10                 |
| `oauth_token`     | POST     | 10                 |
| `batch`           | POST     | 50                 |
| `server_info`     | GET      | 300                |

The current rate-limit status for an App/Endpoint combination is returned with every response with the following headers:

| Header               | Description                                             |
| -------------------- | ------------------------------------------------------- |
| Rate-Limit-Limit     | The number of allowed requests in the current period.   |
| Rate-Limit-Remaining | The number of remaining requests in the current period. |
| Rate-Limit-Reset     | The number of seconds left in the current period.       |

An overview of the rate-limit status can be obtained by querying the [`server_info`](/api-references/server-info.md) endpoint.