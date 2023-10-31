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
  - `transformation` : For all transformations in [Video Transformations](../video-transformation/README.md) except the ones listed above.

## How to Use Pre and Post Transformations?

// TODO:
