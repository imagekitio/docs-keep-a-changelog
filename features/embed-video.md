# Embed video

Embedding a video from ImageKit allows you to integrate the video directly onto your platform without requiring an additional video player. Typically, the code to embed a video appears as:

{% code overflow="wrap" %}
```
<iframe width="560" height="315" src="https://imagekit.io/player/embed/demo/sample-video.mp4?thumbnail=https%3A%2F%2Fik.imagekit.io%2Fdemo%2Fsample-video.mp4%2Fik-thumbnail.jpg" title="ImageKit video player" frameBorder="0" allow="accelerometer; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share; fullscreen"> </iframe>
```
{% endcode %}

[Adaptive bitrate streaming](video-transformation/adaptive-bitrate-streaming.md) (ABS) can be enabled for embedding videos using either either HTTP Live Streaming (HLS) or Dynamic Adaptive Streaming over HTTP (MPEG-DASH) protocol.

### How do you generate embed video code?

Embed video code can be generated for any video from the media library dashboard or from the files details page.

{% hint style="info" %}
The embed feature is exclusive for published video files
{% endhint %}

#### Media library dashboard

To generate the embed video code from the media library dashboard, hover over the lower right section of the video or right-click on the video file and select the "Embed" option.


<div>

<figure><img src="../.gitbook/assets/Screenshot 2023-10-09 at 3.15.42 PM.png" alt=""><figcaption><p>hover video file</p></figcaption></figure>

 

<figure><img src="../.gitbook/assets/Screenshot 2023-10-09 at 3.16.21 PM.png" alt=""><figcaption><p>right-click on video file</p></figcaption></figure>

</div>

#### File detail page

Open the file detail page for any published video file and select the "Embed" option from the header section.

<figure><img src="../.gitbook/assets/Screenshot 2023-10-09 at 3.12.48 PM.png" alt=""><figcaption><p>File detail page</p></figcaption></figure>

### Configure embed video code

Clicking on the "Embed" option with display the embed video model.

<figure><img src="../.gitbook/assets/Screenshot 2023-10-09 at 3.16.41 PM.png" alt=""><figcaption><p>Embed video modal</p></figcaption></figure>

The following embed options are available:&#x20;

#### Expiration time

{% hint style="info" %}
The expiration time option is only available for private video files.
{% endhint %}

By default, embedded video is set to never expire. To specify an expiration date and time, deselect the checkbox.

<figure><img src="../.gitbook/assets/Screenshot 2023-10-09 at 3.17.07 PM.png" alt=""><figcaption><p>Expiration time</p></figcaption></figure>

#### Enable ABS

Select the "Enable ABS" option to activate [Adaptive bitrate streaming](video-transformation/adaptive-bitrate-streaming.md) (ABS) for embedding video. Then, select the desired protocol and resolution option from the dropdown.

<figure><img src="../.gitbook/assets/Screenshot 2023-10-09 at 3.17.43 PM.png" alt=""><figcaption><p>ABS embed options</p></figcaption></figure>

A custom option is also available which lets a user enter a transformation string. This can be used to add multiple [supported video transformation parameters](image-transformations/resize-crop-and-other-transformations.md) that are compatible with [Adaptive bitrate streaming-enabled](video-transformation/adaptive-bitrate-streaming.md) video.

{% hint style="info" %}
The transformation string must contain `sr-<representations>`transformation.
{% endhint %}

Additionally, **Named transformation** can be incorporated as part of custom transformation string by specifying `tr:n-<named_transform>`
