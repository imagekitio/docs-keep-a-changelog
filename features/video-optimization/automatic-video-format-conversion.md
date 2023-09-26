# Automatic video format conversion

With multiple video formats to choose from, it often becomes a difficult task to ensure that the videos on a website are delivered in the right format. Also, it is not easy to encode videos in different codecs. ImageKit's automatic format conversion solves this problem.

## What is Format Optimization?

Format Optimization is the process of delivering the best video format to the end-user while taking into account various factors such as requesting device capabilities, browser support for certain video formats, and your preferences. Ensuring the right format helps you reduce the size of the video and subsequently the playback time.

Today, the most common video codec is H.264. A typical `.mp4` video is usually encoded using H.264 encoder. Almost every device in existence supports this protocol and it’s common for use with online video. However, there are several other codecs available, including MPEG-2, HEVC, VP9, Quicktime, and WMV.

VP9 is an open and royalty-free video coding format developed by Google. It provides excellent compression and is supported by major browsers.

![VP9 support - https://caniuse.com/](<../../.gitbook/assets/image (29).png>)

AV1 (AOMedia Video 1) is a royalty-free video format by the Alliance for Open Media, meant to succeed its predecessor VP9 and compete with the HEVC/H.265 format. Read more about AV1 support across different browsers [here](https://caniuse.com/av1).

ImageKit chooses between H.264, VP9, and AV1 codecs and automatically delivers the video in the appropriate format based on browser support.

Note that the automatic usage of the AV1 codec for output videos is disabled by default. However, you can force the output video to use the AV1 codec by adding `vc-av1` in the transformation string. You can reach out to support@imagekit.io to get this enabled.

The video URL remains the same, but the file is modified. This behavior is transparent for your users. The end result is a small video file and a faster playback time.

{% hint style="info" %}
**Beta feature**
The AV1 codec for output videos is currently in beta.
{% endhint %}

## Frequently asked questions

### How to enable or disable automatic format conversion for my account?

Automatic format optimization is off by default. You can enable or disable automatic format conversion for your account under [video settings](https://imagekit.io/dashboard/settings/videos) in your dashboard.

Select settings from the left main menu. Inside Videos, under optimization, toggle "Use best format for video delivery" option to enable or disable this feature.

![Video settings](<../../.gitbook/assets/video-best-format.png>)

### Can I selectively choose the best format for a particular request?

If you disabled automatic format conversion in the global settings, you can still choose to use this feature for a particular request using `f-auto` parameter.

### How to override the format?

You can use the `f` parameter to override the format for a particular video request. The format that you specify in the `f` parameter will take precedence over any global settings.

#### MP4 format

Use `f-mp4` parameter e.g.

`https://ik.imagekit.io/demo/sample-video.mp4?tr=f-mp4`

![](<../../.gitbook/assets/Screenshot 2021-07-16 at 5.08.09 PM.png>)

#### WebM format

Use `f-webm` parameter e.g.

`https://ik.imagekit.io/demo/sample-video.mp4?tr=f-webm`

![](<../../.gitbook/assets/Screenshot 2021-07-16 at 5.07.53 PM.png>)

####
