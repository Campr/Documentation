# Detached File Collection \(attachments\)

To help with some attachment upload workflows, apps can upload a file before they attach it to a Post. After an upload completes, apps can list their temporary attachments, delete them, or attach them to Posts.

{% method %}
## POST - Upload a single attachment

Upload a file to the detached file storage. The server will keep the file available for attachment to a Post for a specific amount of time that will be provided in the response to the upload request. To set a baseline, The Protocol requires servers to keep detached files for at least **2 hours**.

The type and name of the file can be overriden when attaching to a post.

This endpoint supports [partial uploads](/api-reference/base-concepts#partial-uploads).

#### Request headers

| | Header                | Type                                              | Description                                      |
|-| --------------------- | ------------------------------------------------- | ------------------------------------------------ |
| | `Content-Length`      | Integer                                           | The size of the file being uploaded.             |
|Ø| `Content-Type`        | Mime type (default: `application/octet-stream`)   | The Mime type to associate with this attachment. |
|Ø| `Content-Disposition` | `attachment; filename=&lt;filename&gt;`           | The name of file being uploaded.                 |

#### Response

| Property     | Type                                      | Description                           |
| ------------ | ----------------------------------------- | ------------------------------------- |
| `attachment` | [Attachment](/model-reference/attachment) | Metadata about the newly upload file. |

{% sample lang="http" %}
#### Example request

```
POST /attachments
Content-Length: 1234
Content-Type: image/png
Content-Disposition: attachment; filename=profile.png

<binary content>
```

#### Example response

```
HTTP/1.1 200 OK
Content-Type: application/json
```

```json
{
  "attachment": {
    ...
  }
}
```
{% endmethod %}

{% method %}
## GET/HEAD - List detached files

List all the files that are not currently attached to any Post. As soon as a file is attached to a Post, it disappears from this list. Servers may limit the quantity/size of files being kept around in a detached state. The `server_info` can be queried for detailed information on the quota usage by the current app.

This follows the standard [pagination](/api-reference/base-concepts.md#pagination) format.

#### Response

| Property      | Type                    | Description         |
| ------------- | ----------------------- | ------------------- |
| `attachments` | Array&lt;Attachment&gt; | The detached files. |

{% sample lang="http" %}
#### Example request

```
GET /attachments
```

#### Example response

```
HTTP/1.1 200 OK
Content-Type: application/json
```

```json
{
  "attachments": [{
    ...
  }]
}
```
{% endmethod %}