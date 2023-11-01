---
description: Request transformations on media before or after upload.
---

# Pre and Post Transformation

Using ImageKit.io, you can request a transformation on image or video files when they're uploaded through the [Upload API](../../api-reference/upload-file-api/README.md).

To know more about transformations, see:

- [Image Transformations](../image-transformations/README.md)
- [Video Transformations](../video-transformation/README.md)

## Pre Transformation

A pre-transformation is applied before the uploaded file is saved.

You can only request a single pre-transformation on an asset.

## Post Transformation

A post-transformation is applied once a file has been successfully uploaded & saved.

You can request multiple post-transformations on an asset.

Supported types of post transformation:

- For Images

  - `transformation` : See [Image Transformations](../image-transformations/README.md).
  - `gif-to-video` : (Only applicable for GIF files) See [Gif to MP4](../video-transformation/resize-crop-and-other-common-video-transformations.md#gif-to-mp4).

- For Videos

  - `abs` : See [Adaptive bitrate streaming](../video-transformation/adaptive-bitrate-streaming.md).
  - `thumbnail` : See [Get thumbnail from a video](../video-transformation/resize-crop-and-other-common-video-transformations.md#get-thumbnail-from-a-video).
  - `transformation` : For all transformations in [Video Transformations](../video-transformation/README.md) except ABS & thumbnail.

## How to Use Pre and Post Transformations?

During an [Upload API](../../api-reference/upload-file-api/README.md) call, specify a pre or post transformation under the `transformation` request parameter.

The `transformation` parameter for the Upload API is an object with the following properties:

| Field Name    | Type             | Required                           | Description                                                                                                                                                               |
| ------------- | ---------------- | ---------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| pre           | string           | -                                  | Request parameter for Pre Transformation                                                                                                                                  |
| post          | array of objects | -                                  | Request parameter for Post Transformation                                                                                                                                 |
| post.type     | string           | Yes                                | Type of required post-transformation.<br/>Allowed Values: `abs`, `thumbnail`, `transformation`, `gif-to-video`                                                            |
| post.value    | string           | If `post.type` is `transformation` | Value of required post-transformation.                                                                                                                                    |
| post.protocol | string           | If `post.type` is `abs`            | ABS Streaming Protocol. <br/> Allowed Values: `hls`, `dash`<br/>See [Adaptive bitrate streaming](../video-transformation/adaptive-bitrate-streaming.md) for more details. |

Example of a pre transformation:

```javascript
// Request Body (multipart/form-data) for Upload API
{
  /*
  ...rest of the Upload API request parameters
  */
  "transformation": {
    "pre": "rt-90"
  }
}
```

Example of a request with multiple post transformations:

```javascript
// Request Body (multipart/form-data) for Upload API
{
  /*
  ...rest of the Upload API request parameters
  */
  "transformation": {
    "post": [
        {
          "type": "abs",
          "value: "sr-240_360_480_720_1080",
          "protocol": "dash"
        },
        {
          "type": "transformation",
          "value": "h-300"
        }
    ]
  }
}
```

You can also use pre & post transformations together in a single request. For an example, we have a request with a pre-transformation to change a video's [width](../video-transformation/resize-crop-and-other-common-video-transformations.md#width---w) to "500px" & then eagerly generate a thumbnail for it using a post-transformation.

```javascript
// Request Body (multipart/form-data) for Upload API
{
  /*
  ...rest of the Upload API request parameters
  */
  "transformation": {
    "pre": "w-500",
    "post": [
      {
        "type": "thumbnail"
      }
    ]
  }
}
```

> Note: Don't forget to stringify the contents of the `post` property before making a request.

## Asynchronous behaviour

- Post transformations are always async.
- Pre transformation is sync for image files but async for video files.

## Webhooks

See [Pre and Post Transformation Events](./pre-post-tr-webhook-events.md) for details.
