# Base Concepts

## Response envelope

Because additional metadata is sometime returned alongside the requested content, all responses to list requests come wrapped in a common envelope.

| Property        | Type           | Description                                     |
| --------------- | -------------- | ----------------------------------------------- |
| `posts`         | Array<Post>    | A collection of posts.                          |
| `post`          | Post           | A single post.                                  |
| `post_versions` | Array<Version> | A collection of versions for a specific post.   |
| `links`         | Array<Post>    | Linked-to posts.                                |
| `profiles`      | Array<Profile> | User profiles.                                  |

The `links` and `profiles` properties can be controlled through the `related` query parameter. They content is contextual to the main post/posts.

## Pagination

When the requested collection is too large to fit in a single response, pagination is required to retrieve it in multiple requests.
Every list endpoint returns HTTP headers that contain the pagination information necessary to retrieve the requested collection in its entirety.

Every page will be the same size as the first one.

| Header  | Rel                                 | Description                             |
| ------- | ----------------------------------- | --------------------------------------- |
| `Count` | N/A                                 | Number of posts in the requested range. |
| `Link`  | https://campr.me/rels/first-page    | Link to the first page.                 |
| `Link`  | https://campr.me/rels/previous-page | Link to the previous page.              |
| `Link`  | https://campr.me/rels/next-page     | Link to the next page.                  |
| `Link`  | https://campr.me/rels/last-page     | Link to the last page.                  |

Because they're regular headers, they can be retrieved in a HEAD request if the content is not needed.

## Errors