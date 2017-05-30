Base Layer
==========

The [protocol-name] base layer is common between server-to-server and app-to-server communication. It defines the basic API used by most of the actions in [protocol-name]. It’s the lowest level of abstraction, and the other layers are implemented on top of this one.

Model: the [protocol-post]
--------------------------

A [protocol-post] is to [protocol-name] what a file is to a filesystem. It’s the smallest unit that can be manipulated, and every action on a [protocol-post] is both versioned and atomic. The content of a [protocol-post] is generic JSON data, defined by its type, and will be specific to the applications using it. [protocol-posts] should stay relatively small (<1MB), and larger binary blobs can be attached when needed.

### General structure

```
Todo.
```

### Type and Schema

To ensure compatibility, every JSON [protocol-post] must specify a type, and respect the corresponding [protocol-name] Schema. This ensures that different apps can work seamlessly on the same posts, as long as they’re of a known type.

A type must be a valid HTTPS url, that in turns resolves to a [protocol-name] Schema. A [protocol-name] Schema is a [JSON schema](http://json-schema.org/) with some extra metadata.

Every type is versioned, and published versions are immutable. This allows for simple caching strategies that greatly help with availability (in addition to which [provider-name] also provides a centralized [protocol-post] type repository). Servers are required to ensure that posts respect their advertised schema before accepting them.

Here’s an example of a simple [protocol-name] Schema:

```
Todo.
```

Every type should have the following properties on their root object:

Types can use a special “[protocol-post-link]” format to represent relationships with other [protocol-posts]. These links will be automatically extracted and flattened into the [protocol-post]’s “links” collection. This allows for rich querying while preserving a domain-specific structure within the [protocol-post]’s content.

### Attachments

Larger, unstructured data can be stored as attachment to a [protocol-post]. In this case, there is no size limits other than imposed by the [protocol-name] provider. This is perfect for storing pictures, videos, regular files, etc.

There is no arbitrary limit to how many attachments a single [protocol-post] can have.

Attachments have the following properties:
	digest
	name
	mime type
	size

### [protocol-post] link

Links represent a relationship between a specific [protocol-post] and a specific user, [protocol-post], or [protocol-post] version.

Linking a different user, or one of their [protocol-post]/[protocol-post] version will result in the [protocol-post] being transmitted to them after being created/updated. For this reason, there’s an upper limit of 100 links per [protocol-post].

Links have the following properties:
	entity: Entity
	previous_entity: Entity
	[protocol-post]: Number
	[protocol-post-version]: Digest

Linked [protocol-posts] can be fetched alongside the [protocol-post].

Entities
--------

Users of [protocol-name] are identified on the network by their entity. An entity is a HTTPS url that is unique to that user.

It’s possible for a user to change entity. In that case, all the [protocol-posts] created before that date will be updated to have both the old (original_entity) and new (entity) entities.

Any HTTPS url will work as an entity, but for practical reasons we recommend to keep it as simple as possible (no path and query).

Discovery & Meta [protocol-post]
--------------------------------

A user entity is not enough to communicate with that user’s server. In order to do that, the discovery process will resolve an entity into the Meta [protocol-post] for this user.

The Meta [protocol-post] is a reserved [protocol-post] type that every user has one instance of. It contains the list of servers responsible for managing the [protocol-posts> of the user. The servers are listed by order of priority: the first one should always be used first, and if unavailable then the next, and so on. A `server` contains the various endpoints in the [protocol-name] API. We’ll go through these in more details in the next section.

In all aspects, the Meta [protocol-post] is a standard [protocol-post], and the same APIs can be used to interact with it as with any other [protocol-post]. Many aspects of [protocol-name] are implemented on top of the [protocol-post] system. This allows for simpler and more generic APIs.

Here’s the schema for the Meta [protocol-post]:

```
TODO.
```

HTTP API
--------

Every action on [protocol-posts] happens through the [protocol-name] HTTP API. Each of the following endpoints are found in the Meta [protocol-post], and can point to completely different domains/urls.
Common list concepts

### Pagination

All lists support pagination. The response to a list request contains a set of <Link> headers leading to the other pages. Links to the current page, and pages that do not exist are omitted. A <Count> header is also provided.

```
TODO: Example list response.
```

List endpoints also support HEAD requests, and all headers will be returned in the same way.

### Response envelope

Because additional metadata is sometime returned alongside the requested content, all responses to list requests come wrapped in a common envelope.

```
TODO: list GET response envelope.
```

The `_related_posts` property will contain posts that were linked to in the requested content. What related posts are returned can be controlled using the `related` and `max_related` arguments.

### new_[protocol-post]

Creating a new [protocol-post] is the most basic action in [protocol-name]. It’s as simple as a single HTTP request.

```
TODO: Example POST request.
```

In return, the server will send the newly created [protocol-post], with the addition of the “links” array generated from the content.

```
TODO: Example response.
```

If the response is not needed, the TODO header can be specified. In that case, the response will be limited to this:

```
TODO: Example 204 response.
```

### [protocol-posts]

List the [protocol-posts] for our user. This is the main API endpoint used to retrieve data from a [protocol] server as it accepts many filtering arguments to find specific content.

```
TODO: Example GET basic request.
```

This endpoint supports the following arguments:
	related: profiles | links.
	max_related: 0-500.

	limit: 1-100.
	sort_by: received_at | published_at | version.received_at | version.published_at
	since: unix timestamp.
	until: unix timestamp.
	before: unix timestamp.
	types: or.
	entities: or.
	links: entity/post, and/or.2

```
TODO: Example GET advanced request.
```

```
TODO: Example GET pagination request.
```

Note: pagination occurs *within* the specified date range.

### protocol-post

#### GET (Accept: [ct-protocol-post>]

Retrieve a specific version of the specified post, or the latest if none is specified.

TODO: Example post response.

This endpoint also supports the following arguments:
	related: profiles | links.
	max_related: 0-500 (default 50).

#### PUT

Create a new version of this [protocol-post]. It’s similar to a new_[protocol-post] request, except that a parent version can be specified.

```
TODO: Example PUT request.
```

Note: If no parent version is provided, the latest version will be used as parent.

```
TODO: Example response.
```

If the response is not needed, the TODO header can be specified. In that case, the response will be limited to this:

```
TODO: Example 204 response.
```

#### DELETE

Delete the specified post. The server will create a new version of the post with type DELETE.

#### GET (Accept: [ct-protocol-post-version>])

Retrieve a list of versions for the specified post.
```
TODO: Example version list response.
```

### batch

Relevant information can sometimes be spread among multiple posts. For this reason all API requests can be batched and processed in just one request. A batch request is a multipart HTTP request where each "part" is uniquely identified. The same identifiers are used in the response to put the pieces back together.

Updates within a batch request (POST, PUT, DELETE) are guaranteed to be executed sequentially.

```
TODO: Example batch request + response.
```

[protocol] servers should provide high-performance batch capabilities, and app developers are encouraged to extensively take advantage of this.

Streaming API
-------------

The `streaming` endpoint is very similar to the `[protocol-posts]` GET endpoint. It accepts all of the same arguments and if matching posts are found, a number of them up to [limit] will be sent back right away. After that, new matching posts will be sent as soon as they're received.

```
TODO: Example EventSource registration.
```

Authentication
--------------

[protocol] uses the Hawk 1.1 HTTP authentication scheme. It's built around HMAC digests of requests and responses. A secret is shared with the client during initial authentication, and is then used in client requests to build the **Authorization** header and in server responses to build the **Server-Authorization** header.

#### Request header

#### Response header

#### Payload validation

#### Timestamp skew

#### URL signing

#### Credentials post

The shared secret used to authenticate requests and responses is stored on the server as a standard [protocol-post]. The ID of the post is the `Key ID` in Hawk. [procotol-name] only supports the `sha256` hash algorithm, for both HAMC and payload validation.

Here's the schema for a Credentials [protocol-post]:

```
TODO.
```

CORS
----

Because [protocol] apps can be browser-based, and hosted on different domains, all [protocol] servers should allow CORS for all of the previously described endpoints. OPTIONS requests are therefore supported, and should return the maximum `Access-Control-Max-Age`.

Social Layer
============

The [protocol-name] Social Layer is used for server-to-server communication. It sits on top of the Base Layer and represents the interactions between [protocol-name] users.

Model
-----

### Relationships

A relationship [protocol-post] represents an external relationship with a different [protocol-name] user. It will be used by the [protocol-name] server every time it needs to communicate with the server of the remote user.

The remote user will also have a corresponding relationship post mirroring the one we have for our user.

Here's the schema for a Relationship [protocol-post]:

```
TODO.
```

### Subscriptions

A subscription [protocol-post] represents the intent from a [protocol-name] user to be notified when new posts of a given type are created by another external user.

Here's the schema for a Subscription [protocol-post]:

```
TODO.
```

### Groups.

```
TODO.
```

Notifications
-------------

A [protocol-name] server is responsible for notifying other servers of the [protocol-posts] created by its user. Other servers need to be notified if:

- Their user was linked to in the [protocol-post].
- A subscription from that user exists for this type of [protocol-post].

Of course, a notification should only be sent if the [protocol-post] is public, or if the target user has explicit access to it.

A notification is a request to the `new_[procotol_post]` endpoint. Unless its part of the "new relationship flow", the request should be authenticated with the relationship credentials.

Flow
----

The first time that a [protocol-name] user wishes to link or subscribe to a new external user, a relationship needs to be established between them. Here's how this works.

1. A relationship [protocol-post] is created for user #1. It links to user #2.

```
TODO: Example relationship post.
```

A new credentials [protocol-post] is also created. It links to the relationship [protocol-post].

```
TODO: Example credentials post.
```

2. The relationship [protocol-post] is sent to the server for user #2 through a notification request. This request also includes a `<link>` header to the credentials [protocol-post]. This is a signed URL that can be retrieved without authentication.

```
TODO: Example notification request with link.
```

3. The remote server creates its own copy of the relationship [protocol-post], linking to the one created in #1.

```
TODO: Example remote relationship post.
```

A new credentials [protocol-post] is also created. It links to the relationship [protocol-post].

```
TODO: Example credentials post.
```

The response to the notification request contains a `<link>` header to the credentials [protocol-post], in a similar fashion to what was done in #2.

```
TODO: Example notification response with link.
```

4. The server for user #1 creates a new version of its relationship [protocol-post] with a link to its counterpart.

```
TODO: Example new relationship post.
```

Each server can now use the credentials received from the other to authenticate requests made against it.

App Layer
=========

The [protocol-name] App Layer is used for app-to-server communication. In this context, an app is any piece of software independent from the server, acting on [protocol-posts] on behalf of the user.

The scope for apps is very broad, and encompases everything from small programs on individual IoT devices, to large-scale web services like email bridges, including day-to-day smartphone apps.

Model
-----

### App

Represents a type of external party that the user can allow to act on a specific subset of its posts. Publishers can add optional details to their app to help users identify what it does.

An app also specifies the types of posts it wants to access, along with the permissions it wants to have over these. It can also specify a webhook notification endpoint that will be used to push new content to the app.

Here's the schema for an App [protocol-post]:

```
TODO.
```

### App Authorization

A grant of permissions to a specific client for a given app. This allows for multiple clients to use the same app in parallel, and for them to be individually revoked if necessary.

The permissions on the Authorization [protocol-post] can differ from the ones specified on the app. It's validity can also be limited in time.

Here's the schema for an Authorization [protocol-post]:

```
TODO.
```

Authentication
--------------

Apps need to authenticate with the [protocol-name] server to obtain the credentials needed to perform actions on behalf of the user.

### Discovery

After the user specifies its entity, the client will go through the Discovery process to retrieve the Meta [protocol-post], and with it the various HTTP endpoints to use for OAuth.

```
TODO: Example shortened Meta post.
```

### Register the app

The is only needed once, and will make the client's app known to the [protocol-name] server.

```
TODO: App registration request.
```

A new Credentials [protocol-post] is also created. It links to the App [protocol-post]. The response to the App registration also includes a `<link>` header to the Credentials [protocol-post]. This is a signed URL that can retrieved without authentication.

The App [protocol-post] can be later updated to include revised details and permissions. The credentials provided during this step should be used for all operations on the App [protocol-post].

### OAuth

App authentication is performed through a familiar OAuth flow. This will result in the server providing a Hawk ID/Key pair that can be used to sign all subsequent requests.

The `oauth_auth` and `oauth_token` endpoints are used for the OAuth 2.0 _Authorization Request_ and _Access Token Request_ respectively. [protocol-name] only supports the Authorization Code grant type.

To authorize the app, redirect the user to the `oauth_auth` endpoint with the `client_id` URL parameter set to the `id` of the App [protocol-post]. If this is a web app, a `state` parameter should also be specified to verify the origin of the request when the user is redirected back.

```
TODO: Example oauth_auth request.
```

After the user authorizes the application, they will be redirected to the app's `redirect_uri`. The URL will include a `code` parameter, and a `state` parameter if provided in the initial request. The `code` should then be traded using the `oauth_token` endpoint for the Authorization credentials used to interact with the server. This request should be authenticated with the App credentials.

```
TODO: Example oauth_token request + response.
```

Server information
==================

The `server_info` endpoint allows an app to retrieve more details on the [protocol-post] server. This includes the limits in place, the available storage, and general usage stats for the current app + user. Resource-intensive apps may need to check this on a regular basis to prevent interruptions of service.

```json
{
	"storage": {
		"available": 134217728,
		"used": 104857600
	},
	"limits": {
		"[protocol-posts]": {
			"default_posts": 50,
			"max_posts": 250,
			"default_related": 100,
			"max_related": 500
		},
		"[protocol-post]": {
			"default_versions": 50,
			"max_versions": 250,
			"default_related": 100,
			"max_related": 500
		},
		"batch": {
			"max_requests": 100
		}
	},
	"types": {
		"https://protocol.net/types/instant-message": {
			"count": 46
		}
	}
}
```

The doesn’t represent an absolute view of what’s available to the user, but rather the view of the [protocol-name] server exposed to the current app.

Client schemes
==============

For more compability, apps can register standard schemes for specific [protocol-name] actions that they know how to handle. This can be helpful to handoff workloads from one app to another on native platforms when needed.

Example: A "drive" type application could use a scheme to open an Image [protocol-post] in a more qualified "image editor" app.