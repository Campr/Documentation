# API Reference

Thanks to it's design centered only one core construct: the post, the {{ protocol_name }} API remains simple. All actions are performed through only a few HTTP calls that we'll detail here. The actual URL templates for these endpoints are found in the Meta Post.

## Posts & Attachments

All of these endpoints share a common [Response Envelope](https://www.gitbook.com/book/campr/api/edit#) when returning JSON content.

[`posts`](/api-reference/posts.md) The collection of posts

[`post`](/post) A single post

[`attachments`](/attachments) The collection of unused attachments

[`attachment`](/attachment) A single attachment

## Authentication

[`oauth_authorize`](/oauth_authorize) Start the OAuth flow

[`oauth_token`](/oauth_token) Exchange an OAuth code for the secret token

## Utility

[`batch`](/batch) Perform multiple actions in one request

[`server_info`](/server_info) Usage information for the current server

