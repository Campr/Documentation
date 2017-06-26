# Single Attachment \(attachment\)

The `attachment` endpoint template has the following paramters:

| | Key      | Description                                                |
|-| -------- | ---------------------------------------------------------- |
| | `digest` | The hex-encoded, SHA3-256 hash of the attachment to fetch. |
|Ø| `entity` | The entity of the author of the post this is attached to.  |
|Ø| `post`   | The ID of the post this is attached to.                    |
|Ø| `version | The version of the post this is attached to.               |

{% method %}
## GET/HEAD - Retrieve a single attachment

Download the binary data for a specific attachment.
The HTTP headers `Content-Type` and `Content-Disposition` will only be provided if this attachment was requested within the context of a specific post, of if it is currently detached.

This endpoint supports resuming downloads thanks to the standard `Range` HTTP header.

#### Request headers

| | Header  | Type                                                   | Description                                   |
|-| ------- | ------------------------------------------------------ | --------------------------------------------- |
|Ø| `Range` | `&lt;unit&gt; &lt;start&gt;-&lt;end&gt;/&lt;total&gt;` | The partial range to be being returned. |

#### Response headers

| | Header                | Type                                                   | Description                                   |
|-| --------------------- | ------------------------------------------------------ | --------------------------------------------- |
| | `Content-Length`      | Integer                                                | Size of the file being returned.              |
|Ø| `Content-Range`       | `&lt;unit&gt; &lt;start&gt;-&lt;end&gt;/&lt;total&gt;` | The partial range of the file being returned. |
|Ø| `Content-Type`        | Mime type                                              | Type of the file being returned.              |
|Ø| `Content-Disposition` | `attachment; filename=&lt;filename&gt;`                | Filename of the file being returned.          |

{% sample lang="http" %}
#### Example request

```
GET /attachments/b3f84daaec10476bad8025974e1696cc
```

#### Example response

```
HTTP/1.1 200 OK
Content-Length: 1234
Content-Type: image/png
Content-Disposition: attachment; filename=profile.png

<attachment binary data>
```
{% endmethod %}

{% method %}
## DELETE - Delete a detached file

Delete the specified file from the [detached file](/api-reference/attachments) list. This can be used if the file will never be attached and is no longer needed. It allows an app to free up space for new files to be uploaded.

{% sample lang="http" %}
#### Example request

```
DELETE /attachments/b3f84daaec10476bad8025974e1696cc
```

#### Example response

```
HTTP/1.1 204 OK
```
{% endmethod %}