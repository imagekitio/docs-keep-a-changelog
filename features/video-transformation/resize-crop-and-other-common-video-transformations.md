---
description: >-
  A comprehensive guide that covers all the commonly used video transformations
  that you will need for your web applications.
---

# Resize, crop, and other common video transformations

## Basic video resizing

{% hint style="info" %}
Check out the [limitations of alpha release](./#limitations-of-the-alpha-release) before using real-time transformations in your application.
{% endhint %}

### Width - (w)

Used to specify the width of the output video. Accepts integer value greater than 1.

When you specify just width, the height is adjusted accordingly to maintain the aspect ratio.

{% tabs %}
{% tab title="Original (1280x720)" %}
[https://ik.imagekit.io/demo/sample-video.mp4](https://ik.imagekit.io/demo/sample-video.mp4)\
\
Original 1280x720 px video.

![](<../../.gitbook/assets/image (43).png>)
{% endtab %}

{% tab title="Width 300px" %}
[https://ik.imagekit.io/demo/sample-video.mp4?tr=w-300](https://ik.imagekit.io/demo/sample-video.mp4?tr=w-300)

Notice that height is automatically adjusted to maintain the aspect ratio.

![](<../../.gitbook/assets/image (52).png>)
{% endtab %}
{% endtabs %}

### Height - (h)

Used to specify the height of the output video. Accepts integer value greater than 1. 

When you specify only height, the width is adjusted accordingly to maintain the aspect ratio.

{% tabs %}
{% tab title="Original" %}
[https://ik.imagekit.io/demo/sample-video.mp4](https://ik.imagekit.io/demo/sample-video.mp4)

Original 1280x720 px video.

![](<../../.gitbook/assets/image (33).png>)
{% endtab %}

{% tab title="Height 300px" %}
[https://ik.imagekit.io/demo/sample-video.mp4?tr=h-300](https://ik.imagekit.io/demo/sample-video.mp4?tr=h-300)

Notice that width is automatically adjusted to maintain the aspect ratio.

![](<../../.gitbook/assets/image (35).png>)
{% endtab %}
{% endtabs %}

## Crop, Crop Modes, and Focus

If only, one of the [height(h)](resize-crop-and-other-common-video-transformations.md#height-h) or [width(w)](resize-crop-and-other-common-video-transformations.md#width-w) is specified, then ImageKit.io adjusts the other dimension accordingly to preserve aspect ratio and no cropping takes place.

But when you specify both [height(h)](resize-crop-and-other-common-video-transformations.md#height-h) and [width(w)](resize-crop-and-other-common-video-transformations.md#width-w) dimensions, you need to choose the right cropping strategy based on your website layout and desired output video.

{% hint style="info" %}
**Tip for choosing the right cropping strategy**:sunglasses:****\
****When choosing among different strategies for cropping, think in terms of your website layout and desired output video dimension.
{% endhint %}

* If you want to preserve the whole video content without any cropping and need the exact same dimensions (height and width) in the output as requested, choose either the [pad resize crop](resize-crop-and-other-common-video-transformations.md#pad-resize-crop-strategy-cm-pad_resize) or [forced crop strategy](resize-crop-and-other-common-video-transformations.md#forced-crop-strategy-c-force).
* If you want to preserve the whole video content without any cropping, but it is okay if one or both the dimensions (height or width) in the output are adjusted to preserve the aspect ratio. Then choose either the [max-size cropping](resize-crop-and-other-common-video-transformations.md#max-size-cropping-strategy-c-at_max) or [min-size cropping](resize-crop-and-other-common-video-transformations.md#min-size-cropping-strategy-c-at_least) strategy.
* If you need the exact same dimensions (height and width) in the output video as requested but it's okay to crop the video to preserve the aspect ratio. Then choose either the [maintain ratio crop](resize-crop-and-other-common-video-transformations.md#maintain-ratio-crop-strategy-c-maintain_ratio) or the [extract crop](resize-crop-and-other-common-video-transformations.md#extract-crop-strategy-cm-extract) strategy. You can combine the extract crop strategy with different [focus](resize-crop-and-other-common-video-transformations.md#focus-fo) values to get the desired result.

### Pad resize crop strategy - (cm-pad_resize)

In the pad resize crop strategy, the output dimension (height and width) is the same as requested, no cropping takes place, and the aspect ratio is preserved. This is accomplished by adding padding around the video to get it to match the exact dimension as requested.

#### Example - All padding on one side

In the examples above, we saw that when the video is padded using the [pad resize crop strategy](resize-crop-and-other-common-video-transformations.md#pad-resize-crop-strategy-cm-pad_resize), the padding is equal on both sides of the video. However, there might be cases where we want all the padding to be added on only one side of the video. This can be done using the [focus (fo)](resize-crop-and-other-common-video-transformations.md#focus-fo) parameter.

{% tabs %}
{% tab title="Default crop (400x200)" %}
[https://ik.imagekit.io/demo/sample-video.mp4?tr=w-400,h-200](https://ik.imagekit.io/demo/sample-video.mp4?tr=w-400,h-200)\
\
The video is 400x200 but it is cropped from all sides to preserve the aspect ratio.

![](<../../.gitbook/assets/image (39).png>)
{% endtab %}

{% tab title="cm-pad_resize (center focus)" %}
[https://ik.imagekit.io/demo/sample-video.mp4?tr=w-400,h-200,cm-pad_resize,bg-F3F3F3](https://ik.imagekit.io/demo/sample-video.mp4?tr=w-400,h-200,cm-pad_resize,bg-F3F3F3)\
\
The video is exactly 400x200 and there is no cropping. Extra padding with [background color](resize-crop-and-other-common-video-transformations.md#background-color-bg) F3F3F3 has been added to get 400x200 output dimensions.

![](<../../.gitbook/assets/image (41).png>)
{% endtab %}

{% tab title="cm-pad_resize (left)" %}
You can also control the focus point using [fo parameter](resize-crop-and-other-common-video-transformations.md#focus-fo) to move the actual content to one side using relative positioning.

We added the `fo-left`. Now, all the padding is on the bottom of the video.

![](<../../.gitbook/assets/image (38).png>)
{% endtab %}
{% endtabs %}

### Forced crop strategy - (c-force)

In a forced crop strategy, the output video's dimension (height and width) is exactly the same as requested, no cropping takes place, but the aspect ratio is not preserved. It forcefully squeezes the original video to get it to fit completely within the output dimensions. 

URL - [https://ik.imagekit.io/demo/sample-video.mp4?tr=w-200,h-200,c-force](https://ik.imagekit.io/demo/sample-video.mp4?tr=w-200,h-200,c-force)\
\
Notice that the aspect ratio is changed and the video looks squeezed.

![](<../../.gitbook/assets/image (34).png>)



### Max-size cropping strategy - (c-at_max)

In the max-size crop strategy, whole video content is preserved without any cropping, the aspect ratio is preserved, but one of the dimensions (height or width) is adjusted.

The output video is less than or equal to the dimensions specified in the URL,i.e., at least one dimension will exactly match the output dimension requested, and the other dimension will be equal to or smaller than the corresponding output dimension requested.

URL - [https://ik.imagekit.io/demo/sample-video.mp4?tr=w-200,h-200,c-at_max](https://ik.imagekit.io/demo/sample-video.mp4?tr=w-200,h-200,c-at_max)

Notice that the aspect ratio is maintained and there is no cropping. But the height is reduced so that the video fits within a 200x200 container.

![](<../../.gitbook/assets/image (30).png>)

### Min-size cropping strategy - (c-at_least)

This strategy is similar to the [max-size cropping](resize-crop-and-other-common-video-transformations.md#max-size-cropping-strategy-c-at_max) strategy, with the only difference being that, unlike the max-size strategy, the output video's diemsnion is equal to or larger than the requested dimensions. One of the dimensions will be exactly the same as what is requested, while the other dimension will be equal to or larger than what is requested.

URL - [https://ik.imagekit.io/demo/sample-video.mp4?tr=w-200,h-200,c-at_least](https://ik.imagekit.io/demo/sample-video.mp4?tr=w-200,h-200,c-at_least)

Notice that the height is 200px as requested, but the width is more than 200px. The aspect ratio is maintained and there is no cropping.\


![](<../../.gitbook/assets/image (47).png>)

### Maintain ratio crop strategy

This is the default crop strategy. If nothing is specified in the URL, this strategy gets applied automatically. In this strategy, the output video's dimension (height and width) is the same as requested, and the aspect ratio is preserved. This is accomplished by resizing the video to the requested dimension and then cropping extra parts to get desired height & width.

{% hint style="info" %}
By default, ImageKit.io crops the video from the center but you can change this using the [focus parameter](../image-transformations/resize-crop-and-other-transformations.md#focus-fo).
{% endhint %}

URL - [https://ik.imagekit.io/demo/sample-video.mp4?tr=w-400,h-200](https://ik.imagekit.io/demo/sample-video.mp4?tr=w-400,h-200)\
\
Notice that the video's dimension matches 400x200 but the content is cropped from all edges i.e by default ImageKit will extract the video from the center. You can change this behaviour using the [focus parameter](resize-crop-and-other-common-video-transformations.md#focus-fo).

![](<../../.gitbook/assets/image (32).png>)

### Extract crop strategy - (cm-extract)

In this strategy, the output video's dimension (height and width) is exactly the same as requested, and the aspect ratio is preserved. In this strategy, instead of trying to resize the video as we did in [maintain ratio](resize-crop-and-other-common-video-transformations.md#maintain-ratio-crop-strategy) strategy, we extract out a region of the requested dimension from the original video.

{% hint style="info" %}
By default, ImageKit.io extracts the video from the center but you can change this using the [focus parameter](resize-crop-and-other-common-video-transformations.md#focus-fo).
{% endhint %}

#### Examples - Center and relative focus

{% tabs %}
{% tab title="Default center extract" %}
URL - [https://ik.imagekit.io/demo/sample-video.mp4?tr=h-300,w-300,cm-extract](https://ik.imagekit.io/demo/sample-video.mp4?tr=h-300,w-300,cm-extract)

A 300x200 part is extracted from the center of the original video.

![](<../../.gitbook/assets/image (50).png>)
{% endtab %}

{% tab title="Relative focus" %}
In the relative method, you can use the [focus (fo) parameter](resize-crop-and-other-common-video-transformations.md#focus-fo) to specify that the extract should be done from, let's say, the bottom of the original video.

Valid relative values for `fo` parameters are - `center`, `top`, `left`, `bottom`, `right`, `top_left`, `top_right`, `bottom_left` and `bottom_right`.

![](<../../.gitbook/assets/image (31).png>)


{% endtab %}
{% endtabs %}

### Focus - (fo)

This parameter can be used along with [pad resize](resize-crop-and-other-common-video-transformations.md#pad-resize-crop-strategy-cm-pad_resize), [maintain ratio](resize-crop-and-other-common-video-transformations.md#maintain-ratio-crop-strategy) or [extract crop](resize-crop-and-other-common-video-transformations.md#extract-crop-strategy-cm-extract) to change the behaviour of padding or cropping. Learn more from the different examples shown in respective sections. 

This parameter can have the following values depending upon where it is being used:

1. `left`, `right`, `top`, `bottom` can be to control the position of padding when used with pad resize. [Learn from example](resize-crop-and-other-common-video-transformations.md#example-all-padding-on-one-side).
2. `center`, `top`, `left`, `bottom`, `right`, `top_left`, `top_right`, `bottom_left` and `bottom_right` can be used to define relative cropping during extract crop. [Learn from examples.](resize-crop-and-other-common-video-transformations.md#examples-center-and-relative-focus)

## Commonly used transformations

### Quality - (q)

ImageKit.io allows you to choose a quality level between `1` and `100`. `1` results in the lowest perceptual quality and smallest file size. `100` results in the highest perceptual quality and biggest file size.

{% hint style="info" %}
Choosing 100 quality will not improve an already low-quality video. However, it will increase the size of the video file. It is recommended to not use a 100 quality setting.
{% endhint %}

**Default Value** - `50`. You can be change [automatic video quality optimization](../video-optimization/quality-optimization.md) setting from the dashboard.

#### Video at quality 90

URL - [https://ik.imagekit.io/demo/sample-video.mp4?tr=h-300,w-300,q-90](https://ik.imagekit.io/demo/sample-video.mp4?tr=h-300,w-300,q-90)

Video size is 3.3MB which is larger than the original 1.1MB video.

![Video at quality 90](../../.gitbook/assets/q-90-3.3mb.png)

#### Video at quality 50

URL - [https://ik.imagekit.io/demo/sample-video.mp4?tr=h-300,w-300,q-50](https://ik.imagekit.io/demo/sample-video.mp4?tr=h-300,w-300,q-50)

Video size is 406KB which is less than half of the original 1.1MB video. File size is reduced by 60% and there is no perceptual change in the output video.

![Video at quality 50](../../.gitbook/assets/q-50-406kb.png)

### Format - (f)

Used to specify the format of the output video. If no output format is specified then based on your settings in the dashboard, ImageKit.io automatically picks the best format for that video request.

Possible values include `auto` ,`mp4` , `webm` , `orig`.

**Default Value** - `auto`. You can disable [automatic video format conversion](../video-optimization/automatic-video-format-conversion.md) from the dashboard settings. 

### Named transformation - (n)

[Named Transformations](../named-transformations.md) are an alias for the entire transformation string. \
\
For example, we can create a named transformation - `video_thumbnail` for a transformation string - `tr:w-300,h-300` and is used like:

URL - [https://ik.imagekit.io/demo/sample-video.mp4?tr=n-video_thumbnail](https://ik.imagekit.io/demo/sample-video.mp4?tr=n-video_thumbnail)

![](<../../.gitbook/assets/image (49).png>)

### Background color - (bg)

It is used to specify the background color in RGB Hex Code (e.g. FF0000). This is usually used with [pad_resize](resize-crop-and-other-common-video-transformations.md#pad-resize-crop-strategy-cm-pad_resize) cropping to control the color of extra background padding.

URL - [https://ik.imagekit.io/demo/sample-video.mp4?tr=w-400,h-200,cm-pad_resize,bg-862C2C](https://ik.imagekit.io/demo/sample-video.mp4?tr=w-400,h-200,cm-pad_resize,bg-862C2C)

![](<../../.gitbook/assets/image (46).png>)

\
**Default Value** -  By default the background is black.

**Possible Values** - Valid RGB Hex Code

### Audio codec - (ac)

To remove the audio channel from a video set `ac-none` parameter.
