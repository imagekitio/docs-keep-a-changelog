# Embed video

An embedded video lets you use the video directly on your platform from Imagkit, without any additional video player. An embed video code would  usually look like this

{% code overflow="wrap" %}
```
<iframe width="560" height="315" src="https://stage.imagekit.io/player/embed/se1fciwn2/test_1.5_Y9p3EI9nq.mp4?updatedAt=1693430496863&ik-s=7e68dc5e139ba18ce8de73da30d7fadc736efbae&thumbnail=https%3A%2F%2Fstage-ik.imagekit.io%2Fse1fciwn2%2Ftest_1.5_Y9p3EI9nq.mp4%2Fik-thumbnail.jpg" title="ImageKit video player" frameBorder="0" allow="accelerometer; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share; fullscreen"> </iframe>
```
{% endcode %}

[Adaptive bitrate streaming](video-transformation/adaptive-bitrate-streaming.md) (ABS) can be enabled  for embedding video  for both HTTP Live Streaming (HLS) and Dynamic Adaptive Streaming over HTTP (MPEG-DASH) protocol

### How do you generate embed video code?

Embed video code could be generated for any video from the media library dashboard and from the files detail page.

{% hint style="info" %}
The embed feature is only available for published video files
{% endhint %}

#### Media library dashboard

To generate embed video code from the media library dashboard hover on the lower right section of the video or right-click on a video file and select the embed option.\


<div>

<figure><img src="../.gitbook/assets/Screenshot 2023-10-09 at 5.16.55 AM.png" alt=""><figcaption><p>hover video file</p></figcaption></figure>

 

<figure><img src="../.gitbook/assets/Screenshot 2023-10-09 at 5.43.10 AM.png" alt=""><figcaption><p>right-click on video file</p></figcaption></figure>

</div>

#### File detail page

Open the file detail page for any published video file and select the embed option from the header section.

<figure><img src="../.gitbook/assets/Screenshot 2023-10-09 at 5.52.53 AM.png" alt=""><figcaption><p>FIle detail page</p></figcaption></figure>

### Configure embed video code

On clicking on the embed option it will open the embed video model.

<figure><img src="../.gitbook/assets/Screenshot 2023-10-09 at 6.07.37 AM.png" alt=""><figcaption><p>Embed video modal</p></figcaption></figure>

The following embed options are available&#x20;

#### Expiration time

{% hint style="info" %}
The expiration time option is only available for private video files.
{% endhint %}

An embedded video is by default set to never expire unselect the check box to select an expiration date and time.

<figure><img src="../.gitbook/assets/Screenshot 2023-10-09 at 6.19.36 AM.png" alt=""><figcaption><p>Expiration time</p></figcaption></figure>

#### Enable ABS

Select the enable ABS option to activate  [Adaptive bitrate streaming](video-transformation/adaptive-bitrate-streaming.md) (ABS) for embedding video then select the protocol and resolution option from the dropdown.

<figure><img src="../.gitbook/assets/Screenshot 2023-10-09 at 6.29.16 AM.png" alt=""><figcaption></figcaption></figure>

The custom option is also available which lets a user enter a transformation string, which could be used to add multiple [supported video transformation parameters](image-transformations/resize-crop-and-other-transformations.md) that are also supported by [Adaptive bitrate streaming-enabled](video-transformation/adaptive-bitrate-streaming.md) video.

{% hint style="info" %}
The transformation string must contain `sr-<representations> transformation.`
{% endhint %}

**Named transformation** could also be added in a custom transformation string as `tr:n-named_transform`\
