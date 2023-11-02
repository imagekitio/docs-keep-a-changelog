---
description: Modify media files before upload. Eagerly generate transformations after upload.
---

# Pre and Post Transformation

Using ImageKit.io, you can specify pre & post transformations on image or video files during upload.

Pre-transformation lets you modify image or video files before they're uploaded.

Post-transformations help you eagerly generate transformations after an image or video file has been uploaded to ensure fast delivery to your users.

To know more about transformations, see:

- [Image Transformations](../image-transformations/README.md)
- [Video Transformations](../video-transformation/README.md)

For complete API reference, refer to [Upload API](../../api-reference/upload-file-api/) docs.

## Pre Transformation

A pre-transformation is applied **before** the file is uploaded to ImageKit's Media Library. You can only request a single pre-transformation on an asset.

Some things you can use a pre-transformation for:

- Features like [PNG compression](../image-optimization/png-compression.md) to upload smaller files & save on storage costs.
- Change the height or width of an image or video file
- Add some text or logo to an image (using [Overlays on Image](../image-transformations/overlay-using-layers.md)) or a video (using [Overlays on Video](../video-transformation/overlay.md)).
  - You can also use this feature to add watermarking.
- [Trim](../video-transformation/resize-crop-and-other-common-video-transformations.md#trimming) a video to a max duration.
- [Crop](../image-transformations/resize-crop-and-other-transformations.md#crop-crop-modes-and-focus) a profile image uploaded by a user.

## Post Transformation

A post-transformation is applied **after** a file has been successfully in the media library. You can request multiple post-transformations on an asset.
These transformations are created in addition to the original asset.

> Note: ImageKit generates transformations when they're accessed for the first time by default.<br/>
> You can use post-transformations to generate these automatically after file upload so that the transformation is ready & available for fast delivery.

Some things you can use post-transformations for:

- Keeping the [thumbnail](../video-transformation/resize-crop-and-other-common-video-transformations.md#get-thumbnail-from-a-video) ready for a video.
- Using [Adaptive bitrate streaming](../video-transformation/adaptive-bitrate-streaming.md) to eagerly generate the best video streams for a wide variety of devices.
- Create images with high [DPR (device pixel ratio)](../image-transformations/resize-crop-and-other-transformations.md#dpr---dpr) for premium mobile devices.

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

> Note: A single pre-transformation is allowed per file but you can always combine multiple transformations (separated by commas) in the pre-transformation string.<br/><br/>
> For an example, if you want to modify a video to have [height](../video-transformation/resize-crop-and-other-common-video-transformations.md#height---h) as 300px, [width](../video-transformation/resize-crop-and-other-common-video-transformations.md#width---w) as 400px & have a 5px red [border](../video-transformation/resize-crop-and-other-common-video-transformations.md#border---b) before it's uploaded, you can specify `h-300,w-400,b-5_red` in the `pre` request parameter.

Example of a request with a pre transformation:

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

You can also use pre & post transformations together in a single request. For an example, we have a request with a pre-transformation to change a video's width to "500px" & then eagerly generate a thumbnail for it using a post-transformation.

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

You can use webhooks to receive success and failure notifications for pre & post-transformations after they complete execution.<br/>
You can integrate these events in your media workflow to ensure that a pre or post transformation happened as requested.<br/>
See [Pre and Post Transformation Events](./pre-post-tr-webhook-events.md) for more details.
