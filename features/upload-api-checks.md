---
description: Run server-side checks before files are uploaded to Media Library.
---

# Upload API Checks

Using ImageKit.io, you can specify checks to run on image or video files before they're uploaded to your Media Library.

For complete API reference, refer to [Upload API](../api-reference/upload-file-api/) docs.

You can perform multiple checks on an asset before uploading it.

You can use Upload API checks to prevent people from uploading files in unsupported formats (using `file.mime`), exceeding size (using `file.size`) or not meeting some other criteria.

## Supported Fields for Upload API Checks

| Field Name                            | Supported Operators               |
| ------------------------------------- | --------------------------------- |
| `request.fileName`                    | `=`, `NOT =`, `:`, `IN`, `NOT IN` |
| `request.useUniqueFileName`           | `=`, `NOT =`                      |
| `request.tags`                        | `HAS`, `NOT HAS`                  |
| `request.folder`                      | `=`, `NOT =`, `:`, `IN`, `NOT IN` |
| `request.isPrivateFile`               | `=`, `NOT =`                      |
| `request.isPublished`                 | `=`, `NOT =`                      |
| `request.customCoordinates`           | `=`, `NOT =`, `:`, `IN`, `NOT IN` |
| `request.webhookUrl`                  | `=`, `NOT =`, `:`, `IN`, `NOT IN` |
| `request.overwriteFile`               | `=`, `NOT =`                      |
| `request.overwriteAITags`             | `=`, `NOT =`                      |
| `request.overwriteTags`               | `=`, `NOT =`                      |
| `request.overwriteCustomMetadata`     | `=`, `NOT =`                      |
| `request.customMetadata.<field_name>` | See the note below                |
| `file.size`                           | `=`, `>`, `>=`, `<`, `<=`         |
| `file.mime`                           | `=`, `NOT =`, `:`, `IN`, `NOT IN` |
| `mediaMetadata.width`                 | `=`, `>`, `>=`, `<`, `<=`         |
| `mediaMetadata.height`                | `=`, `>`, `>=`, `<`, `<=`         |
| `mediaMetadata.duration`              | `=`, `>`, `>=`, `<`, `<=`         |
| `mediaMetadata.videoCodec`            | `=`, `NOT =`, `:`, `IN`, `NOT IN` |
| `mediaMetadata.audioCodec`            | `=`, `NOT =`, `:`, `IN`, `NOT IN` |
| `mediaMetadata.bitRate`               | `=`, `>`, `>=`, `<`, `<=`         |

Note:
To run checks on custom metadata fields, specify the field you want to run the check on in a flat format.
For eg: If you want to run a check on a custom metadata field named `label`, you can specify the field in check as `request.customMetadata.label`.

Custom metadata fields will support operators as per their set type.

| Type           | Supported Operators                                     |
| -------------- | ------------------------------------------------------- |
| Textarea, Text | `=`, `NOT =`, `:`, `IN`, `NOT IN`                       |
| Number         | `=`, `NOT =`, `>`, `>=`, `<`, `<=`                      |
| Date           | `=`, `NOT =`, `:`, `IN`, `NOT IN`, `>`, `>=`, `<`, `<=` |
| Boolean        | `=`, `NOT =`                                            |
| SingleSelect   | `=`, `NOT =`, `:`, `IN`, `NOT IN`, `>`, `>=`, `<`, `<=` |
| MultiSelect    | `HAS`, `NOT HAS`                                        |

You can also combine checks using the `AND` and `OR` operators and group them using paranthesis `()`. For eg.

`"file.mime" : image OR ("mediaMetadata.videoCodec" = hevc AND "mediaMetadata.audioCodec" = aac)` checks if the file's mimetype starts from the text `image` or if it matches the set video and audio codec values.

## How to Use Upload API Checks?

During an [Upload API](../../api-reference/upload-file-api/README.md) call, specify a upload check using the `checks` request parameter.

Example of a request with a check that prevents you from uploading a file to the `marketing` folder.

```javascript
// Request Body (multipart/form-data) for Upload API
{
  /*
  ...rest of the Upload API request parameters
  */
  "checks": '"request.folder" NOT = marketing'
}
```

Example of a request with checks on multiple fields that verifies that the file uploaded fits within the given dimensions.

```javascript
// Request Body (multipart/form-data) for Upload API
{
  /*
  ...rest of the Upload API request parameters
  */
  "checks": '"mediaMetadata.width" <= 1500 AND "mediaMetadata.height" <= 2000'
}
```

> Don't forget to stringify the contents of the `checks` field before making a request.
> Also remember to quote the fields you're running checks on properly.

## Asynchronous behaviour

For requests involving pre-transformations on video files, checks will be async.

## Webhooks

You'll get an [upload.pre-transform.error](./pre-post-transformation/pre-post-tr-webhook-events.md#uploadpre-transformerror) webhook event in case the check fails or an error occurs.
