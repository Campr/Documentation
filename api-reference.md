# API Reference

With only one core construct: **The {{ book.protocol-post }}**, the {{ book.protocol-name }} API remains simple. All actions are performed through only a few HTTP calls that we'll detail here. The actual URL templates for these endpoints are found in the Meta Post.

## Core

All of these endpoints share a common [Response Envelope](https://www.gitbook.com/book/campr/api/edit#) when returning JSON content.

[`posts`](/api-reference/posts.md) The collection of posts

[`post`](/api-reference/post.md) A single post

[`attachments`](/api-reference/attachments.md) The collection of unused attachments

[`attachment`](/api-reference/attachment.md) A single attachment

## Authentication

[`oauth_authorize`](/api-reference/oauth_authorize.md) Start the OAuth flow

[`oauth_token`](/api-reference/oauth_token.md) Exchange an OAuth code for the secret token

## Utility

[`batch`](/api-reference/batch.md) Perform multiple actions in one request

[`server_info`](/api-reference/server_info.md) Usage information for the current server

