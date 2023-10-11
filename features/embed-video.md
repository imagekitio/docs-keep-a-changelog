# Embed video

Embedding a video from ImageKit allows you to integrate the video directly onto your platform without requiring an additional video player. With this capability, you can leverage:

* [**Video transformations**](video-transformation/)**:** Apply unique video transformations when to videos embedded using Adaptive Bitrate Streaming (ABS) to suit your platform's design needs.
* [**Adaptive bitrate streaming**](video-transformation/adaptive-bitrate-streaming.md): Benefit from comprehensive support for both HLS and MPEG-DASH. This feature ensures automatic generation of the necessary streaming representations and their related files.
* **Plug-and-Play**: The embedded videos are ready for immediate playback, which means there's no need for additional integrations or players.
* **Smooth Playback**: Viewers can expect a smooth start and consistent playback, ensuring a premium viewing experience.
* **Video Thumbnail**: Showcase a preview image or thumbnail before the video begins, enhancing user engagement and anticipation.

Typically, the code to embed a video appears as:

{% code overflow="wrap" %}
```
<iframe width="560" height="315" src="https://imagekit.io/player/embed/demo/sample-video.mp4?thumbnail=https%3A%2F%2Fik.imagekit.io%2Fdemo%2Fsample-video.mp4%2Fik-thumbnail.jpg" title="ImageKit video player" frameBorder="0" allow="accelerometer; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share; fullscreen"> </iframe>
```
{% endcode %}

### How to Generate and Configure the Embed Video Code?

Embed video code can be generated for any video from the media library dashboard and from the files detail page.

To begin, head to either the media library dashboard or the file detail page of your chosen published video.

{% hint style="info" %}
The embed feature is exclusive for published video files
{% endhint %}

To generate the embed video code from the media library dashboard, hover over the lower right section of the video or right-click on the video file and select the "Embed" option.

In the Media Library, hover over the desired video or right-click on it, then select the "Embed" option. On the file detail page, the "Embed" option is conveniently located in the header section.

<div data-full-width="false">

<figure><img src="../.gitbook/assets/Screenshot 2023-10-09 at 3.15.42 PM.png" alt=""><figcaption><p>hover video file</p></figcaption></figure>

 

<figure><img src="../.gitbook/assets/Screenshot 2023-10-09 at 3.16.21 PM.png" alt=""><figcaption><p>right-click on video file</p></figcaption></figure>

</div>

<figure><img src="../.gitbook/assets/Screenshot 2023-10-09 at 3.12.48 PM.png" alt=""><figcaption><p>File detail page</p></figcaption></figure>

Upon selecting "Embed", a modal will appear with various configuration options, including the embed iframe code.

<figure><img src="../.gitbook/assets/Screenshot 2023-10-09 at 3.16.41 PM.png" alt=""><figcaption><p>Embed video modal</p></figcaption></figure>

The following embed options are available:

#### Expiration time

{% hint style="info" %}
The expiration time option is only available for private video files.
{% endhint %}

By default, embedded video is set to never expire. To specify an expiration date and time, deselect the checkbox.

<figure><img src="../.gitbook/assets/Screenshot 2023-10-09 at 3.17.07 PM.png" alt=""><figcaption><p>Expiration time</p></figcaption></figure>

#### Enable ABS

You have the option to enable [Adaptive bitrate streaming](embed-video.md#enable-abs) (ABS) and embed the videos using either HTTP Live Streaming (HLS) or Dynamic Adaptive Streaming over HTTP (MPEG-DASH) protocols. To do so, simply select the "Enable ABS" option, and then choose your preferred protocol and resolution from the dropdown menu.

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption><p>ABS embed options</p></figcaption></figure>

A custom option is also available which lets a user enter a transformation string. This can be used to add multiple [supported video transformation parameters](image-transformations/resize-crop-and-other-transformations.md) that are compatible with [Adaptive bitrate streaming-enabled](video-transformation/adaptive-bitrate-streaming.md) video.

{% hint style="info" %}
The transformation string must contain `sr-<representations>`transformation.
{% endhint %}

Additionally, **Named transformation** can be incorporated as part of custom transformation string by specifying `tr:n-<named_transform>`
