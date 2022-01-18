---
description: URL-based parameters to manipulate videos in real-time.
---

# Video Transformation (Beta release)

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
  ****[https://ik.imagekit.io/demo/sample-video.mp4](https://ik.imagekit.io/demo/sample-video.mp4)
* **Resized 300x300 video**\
  ****[https://ik.imagekit.io/demo/`tr:w-300,h-300`/sample-video.mp4](https://ik.imagekit.io/demo/tr:w-300,h-300/sample-video.mp4)

These transformation parameters `w-300,h-300` can be added in the URL as path params or as query parameters.

* **As path parameter** [https://ik.imagekit.io/demo/`tr:w-200,h-200`/sample-video.mp4](https://ik.imagekit.io/demo/tr:w-200,h-200/sample-video.mp4)
* **As query parameter** [https://ik.imagekit.io/demo/sample-video.mp4?`tr=w-200,h-200`](https://ik.imagekit.io/demo/sample-video.mp4?tr=w-200,h-200)

## Limitations of the Beta release

* Input video upto `300MB` in size is supported for transformations. This limit can be adjusted based on your pricing plan.
* When you request a new transformation or have turned on video optimization features, if the video is not cached on CDN or our internal caches, ImageKit will transform the video in real-time. However if the video file is being downloaded from your origin takes more than 15 seconds, ImageKit will give a 302 and serve the original content. Within a few seconds, optimized transformations are generated and stored in our caches. From that point onwards, we will serve the actual transformed video.