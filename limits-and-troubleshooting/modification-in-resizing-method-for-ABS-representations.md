---
description: >-
  ABS resizing parameter changelog
---

# Modifications in the resizing method for ABS representations

{% hint style="info" %}
All ABS representations created after 25th April, 2023 will use [at_max cropping strategy](https://docs.imagekit.io/features/video-transformation/resize-crop-and-other-common-video-transformations#max-size-cropping-strategy-c-at_max) to resize the representations instead of current [pad_resize crop mode](https://docs.imagekit.io/features/video-transformation/resize-crop-and-other-common-video-transformations#pad-resize-crop-strategy-cm-pad_resize).
{% endhint %}

As of 23 March 2023, Imagekit resizes the ABS representations using [pad_resize crop mode](https://docs.imagekit.io/features/video-transformation/resize-crop-and-other-common-video-transformations#pad-resize-crop-strategy-cm-pad_resize), to a strict aspect ratio of 16:9 for all the resolutions. This adds black color padding for inputs that don't have aspect ratio of 16:9.

For playlist URL <https://ik.imagekit.io/demo/sample-portrait.mp4/ik-master.mpd?tr=sr-240_360_480_720> as of 23 March 2023:

![ABS with black padding](../.gitbook/assets/screenshot-3-march-2023-abs-with-black-padding.png)

After the planned modifications on 25th April 2023, Imagekit will use [at_max cropping strategy](https://docs.imagekit.io/features/video-transformation/resize-crop-and-other-common-video-transformations#max-size-cropping-strategy-c-at_max) to resize the representations. This will preserve the original aspect ratio of input and not add padding to the output.

For playlist URL <https://ik.imagekit.io/demo/sample-portrait.mp4/ik-master.mpd?tr=sr-240_360_480_720> after modifications:

![ABS retains aspect ratio of original input](../.gitbook/assets/screenshot-3-march-2023-abs-retains-aspect-ratio-of-original-input.png)

## Why is this behavior changing?

Adding black padding to the output restricted the experience of portrait videos on portrait screens.

Mobile devices in portrait mode would not utilize their entire screen area efficiently, as the 16:9 ratio will restrict the player to a small portion of the screen & the black padding further reduces the size of the actual video content being rendered.

![Comparison of ABS playlists for a portrait video played on a portrait screen](../.gitbook/assets/screenshot-3-march-2023-potrait-screen-playing-potrait-video.png)

## Handling this change

So far, Imagekit’s ABS has ensured that the output always has an aspect ratio of 16:9, irrespective of the input’s dimensions. In case you have designed your user interface such that it relies on the dimension of the output video content, then you will have to make changes so that the user interface is not dependent on the final output dimension. Once this change is live, the output will have the aspect ratio of the input video.

### With video.js

If you want to preserve the 16:9 aspect ratio of the play, you can consider using the aspect-ratio setting in the video.js layout options.

```javascript
videojs('my-player', {
    controls: true,
    aspectRatio: "16:9" // Set aspect ratio that suits your UI
});
```

[Learn more in video.js docs](https://videojs.com/guides/layout/#setting-aspect-ratio)
