---
description: >-
  Adaptive bitrate streaming in ImageKit
---

# Adaptive bitrate streaming

Adaptive bitrate streaming (ABS) enables the optimum streaming video viewing experience for different types of devices over a broad set of connection speeds. This results in very little buffering, a fast start time and a good experience for both high-end and low-end connections.

The client (e.g. a video player) loads the manifest file and then chooses an appropriate segment, usually starting from the lowest bit rate stream to speed up initial playback. If network throughput is greater than the bit rate of the downloaded segment, it requests a higher bit rate segment. The client continues to adapt to changes in network throughput to ensure a smooth playback experience. The exact algorithm of choosing which segment to load can vary from client to client but fundamentally it remains the same -- choose the appropriate segment and adapt as network (or other device constraints) changes.

For ABS to work, the client needs a manifest file that contains information about segments of different variants at varying bitrates. ImageKit can generate and deliver all necessary variants and manifest files from a single source video that is accessible through your ImageKit's account. The original video can be hosted in the ImageKit [media library](../media-library/README.md) or [external storage](../integration/configure-origin/README.md) integrated with ImageKit. Extra storage created because of generated variants and manifest files is counted towards your media library storage.

ImageKit supports the following streaming protocols. Both leverage existing HTTP infrastructure including CDN caching.

* HTTP Live Streaming (HLS)
* Dynamic Adaptive Streaming over HTTP (MPEG-DASH) protocol

# Live demo
See the live demo along with code examples here - https://imagekit.io/use-cases/adaptive-bitrate-streaming-videos/

## HTTP Live Streaming (HLS)
The following URL will generate HLS manifest and five variants at differernt resolutions i.e. 240p, 360p, 480p, 720p and 1080p.

```markup
https://ik.imagekit.io/demo/sample-video.mp4/ik-master.m3u8?tr=sr-240_360_480_720_1080
```

## Dynamic Adaptive Streaming over HTTP (MPEG-DASH) protocol
The following URL will generate DASH manifest and five variants at differernt resolutions i.e. 240p, 360p, 480p, 720p and 1080p.

```markup
https://ik.imagekit.io/demo/sample-video.mp4/ik-master.mpd?tr=sr-240_360_480_720_1080
```

{% hint style="info" %}
The first time you access the manifest URL, a `202` HTTP status code is returned with an empty response. In the background, ImageKit will generate the required variants as per the value of `sr`. It could take up to a minute or two for this to finish. This also depends on video file size and your origin location. The next time you request the same URL, a manifest file is returned in the response with a `200` status code.
{% endhint %}


## Representations
Use `sr-<representations>` transformation to generate the master manifest file for DASH along with necessary variants and segments.

`representations` is the list of different representations you want to create separated by an underscore `_`.

### List of supported representations

`sr-<representations>` transformation is used to specify representations you want to create separated by an underscore `_`. Here is the list of supported representations for both HLS and DASH.

| Representations | Resolution | Max Bitrate | Video Codec | Audio Codec | Format |
|-----------------|------------|-------------|-------------|-------------|--------|
| 240             | 426x240    | 200K        | h264        | aac         | mp4    |
| 360             | 640x360    | 400K        | h264        | aac         | mp4    |
| 480             | 854x480    | 800K        | h264        | aac         | mp4    |
| 720             | 1280x720   | 3300K       | h264        | aac         | mp4    |
| 1080            | 1920x1080  | 5500K       | h264        | aac         | mp4    |
| 1440            | 2560x1440  | 12000K      | h264        | aac         | mp4    |
| 2160            | 3840x2160  | 18000K      | h264        | aac         | mp4    |


{% hint style="info" %}
ImageKit uses H.264 codec for encoding video and AAC for encoding audio for both HLS and DASH.
{% endhint %}

{% hint style="info" %}
**Important note for existing users**
All ABS representations created after 25th April, 2023 will use [at_max cropping strategy](https://docs.imagekit.io/features/video-transformation/resize-crop-and-other-common-video-transformations#max-size-cropping-strategy-c-at_max) to resize the representations instead of current [pad_resize crop mode](https://docs.imagekit.io/features/video-transformation/resize-crop-and-other-common-video-transformations#pad-resize-crop-strategy-cm-pad_resize). Output will match the aspect ratio of the original video after this change. [Learn more](../../limits-and-troubleshooting/modification-in-resizing-method-for-ABS-representations.md)
{% endhint %}

## Transformation

You can transform the final video using any [supported video transformation parameter](../video-transformation/resize-crop-and-other-common-video-transformations.md) in ImageKit except `w`, `h`, `ar`, `f`, `vc`, `ac`, and `q`.
