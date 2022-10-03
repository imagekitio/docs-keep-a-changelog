---
description: URL-based parameters to manipulate videos in real-time.
---

# Video Transformation

## URL Based transformations

Using ImageKit.io, you can perform real-time transformations to deliver perfect videos to the end-users. These transformations can be performed by using the URL-based transformation [parameters](../image-transformations/resize-crop-and-other-transformations.md). 

{% tabs %}
{% tab title="URL structure" %}
```markup
        URL-endpoint        transformation   video path                                    
┌──────────────────────────┐┌────────────┐┌───────────────┐
https://ik.imagekit.io/demo/tr:w-300,h-300/sample-video.mp4
```
{% endtab %}
{% endtabs %}

* **Original video**\
  [https://ik.imagekit.io/demo/sample-video.mp4](https://ik.imagekit.io/demo/sample-video.mp4)
* **Resized 300x300 video**\
  [https://ik.imagekit.io/demo/`tr:w-300,h-300`/sample-video.mp4](https://ik.imagekit.io/demo/tr:w-300,h-300/sample-video.mp4)

These transformation parameters `w-300,h-300` can be added in the URL as path params or as query parameters.

* **As path parameter** [https://ik.imagekit.io/demo/`tr:w-200,h-200`/sample-video.mp4](https://ik.imagekit.io/demo/tr:w-200,h-200/sample-video.mp4)
* **As query parameter** [https://ik.imagekit.io/demo/sample-video.mp4?`tr=w-200,h-200`](https://ik.imagekit.io/demo/sample-video.mp4?tr=w-200,h-200)

## Limitations

* Input video upto `300MB` in size is supported for transformations. This limit can be adjusted based on your pricing plan.
* When you request a new transformation or have turned on video optimization features, if the video is not cached on CDN or our internal caches, ImageKit will transform the video in real-time. However if the video file is being downloaded from your origin takes more than 15 seconds, ImageKit will give a 302 and serve the original content. Within a few seconds, optimized transformations are generated and stored in our caches. From that point onwards, we will serve the actual transformed video.

### Inputs for video optimization

VideoAPI supports the following combinations of input containers and video codecs for optimization.

| Container format | Video codec            | Audio codec |
| ---------------- | ---------------------- | ----------- |
| mp4              | h264, av1              | aac, opus   |
| webm             | vp8, vp9, av1          | opus        |
| mpeg             | mpeg1video, mpeg2video | mp2         |
| 3gp              | h263                   | aac         |
| ogg              | theora                 | vorbis      |

## Pricing
Every new video transformation that has never been done before will contribute toward video processing units using the below definition. Subsequent views of the same video transformation only count towards bandwidth. 

Video processing units used depends on output video duration and resolution:

* 1 second of SD video output = 1 unit
* 1 second of HD video output = 2 units
* 1 second of 4K video output = 4 units
* 1 second of 8K video output = 8 units

Special operations:

* **Audio extraction** - Using [`vc-none`](./resize-crop-and-other-common-video-transformations.md#video-codec-vc) transformation results in audio output. This operation is equal to processing the input video in SD output resolution for the duration of the output audio.
* **Adaptive bitrate streaming** - This operation is equal to processing a 30 seconds SD resolution video. In addition, all generated representations are charged based on requested resolutions.
* **Get thumbnail** - This operation is equal to processing a 30 seconds SD resolution video.

## Recommendations

Here are a few recommendations you should follow while using video API in a live environment.

### Eagerly generate transformation
Whether a video is optimized or transformed, a new video(s) is generated and saved internally by ImageKit. This process could take a few seconds, depending on the video duration, output format, and transformation parameters. Therefore, you should eagerly generate the transformation before using the video URL in a live environment to ensure it works as expected. It guarantees that all necessary transformations are prepared and ready to be served. This avoids unnecessary data transfer costs on your origin. You can listen to [video webhook events](./video-webhook-events.md) to get updates on asset transformations.

### Check assets in Chrome and Safari
ImageKit prepares a `webm` and `mp4` variant of the video to serve optimized video to your users. In a few cases, for specific input videos, the encoding could permanently fail for one or both formats. In this case, you will always get `302` instead of `200`. Using such a URL in a live environment would result in repeated transformation attempts. This will unnecessarily increase origin data transfer at your end. To ensure that the video is ready in both formats, always fetch the same URL in both Chrome and Safari.
