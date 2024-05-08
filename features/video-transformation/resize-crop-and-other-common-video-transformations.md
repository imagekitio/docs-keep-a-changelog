---
description: >-
  A comprehensive guide that covers all the commonly used video transformations
  that you will need for your web applications.
---

# Resize, crop, and other common video transformations

## Basic video resizing

### Width - (w)

Used to specify the width of the output video. Accepts integer value greater than 1.

You can also provide an arithmetic expression such as `iw_div_2`, `cw_mul_0.8`, or `ih` as a value for this parameter. Doing so will first calculate the value of the expression and then apply transformations using that value. Learn more about arithmetic expressions [here](../arithmetic-expressions-in-transformations.md).

When you specify just width, the height is adjusted accordingly to maintain the aspect ratio.

{% tabs %}
{% tab title="Original (1280x720)" %}
[https://ik.imagekit.io/demo/sample-video.mp4](https://ik.imagekit.io/demo/sample-video.mp4)\
\
Original 1280x720 px video.

![](<../../.gitbook/assets/image (42).png>)
{% endtab %}

{% tab title="Width 300px" %}
[https://ik.imagekit.io/demo/sample-video.mp4?tr=w-300](https://ik.imagekit.io/demo/sample-video.mp4?tr=w-300)

Notice that height is automatically adjusted to maintain the aspect ratio.

![](<../../.gitbook/assets/image (41).png>)
{% endtab %}
{% endtabs %}

### Height - (h)

Used to specify the height of the output video. Accepts integer value greater than 1.&#x20;

You can also provide an arithmetic expression such as `ih_div_2`, `ch_mul_0.5`, or `ih` as a value for this parameter. Doing so will first calculate the value of the expression and then apply transformations using that value. Learn more about arithmetic expressions [here](../arithmetic-expressions-in-transformations.md).

When you specify only height, the width is adjusted accordingly to maintain the aspect ratio.

{% tabs %}
{% tab title="Original" %}
[https://ik.imagekit.io/demo/sample-video.mp4](https://ik.imagekit.io/demo/sample-video.mp4)

Original 1280x720 px video.

![](<../../.gitbook/assets/image (43).png>)
{% endtab %}

{% tab title="Height 300px" %}
[https://ik.imagekit.io/demo/sample-video.mp4?tr=h-300](https://ik.imagekit.io/demo/sample-video.mp4?tr=h-300)

Notice that width is automatically adjusted to maintain the aspect ratio.

![](<../../.gitbook/assets/image (44).png>)
{% endtab %}
{% endtabs %}

### Aspect ratio - (ar)

Used to specify the aspect ratio of the output video. It is the ratio of width to height of the output video. This parameter must be used along with either the [height(h)](resize-crop-and-other-common-video-transformations.md#height-h) or [width(w)](resize-crop-and-other-common-video-transformations.md#width-w) parameter but not both.

You can also provide an arithmetic expression such as `iar_div_2`, or `car_mul_0.75` as a value for this parameter. Doing so will first calculate the value of the expression and then apply transformations using that value. Learn more about arithmetic expressions [here](../arithmetic-expressions-in-transformations.md).

Usage = `ar-<width>-<height>`

{% hint style="info" %}
If you specify both [height(h)](resize-crop-and-other-common-video-transformations.md#height-h) and [width(w)](resize-crop-and-other-common-video-transformations.md#width-w) in the URL along with [aspect ratio(ar)](resize-crop-and-other-common-video-transformations.md#aspect-ration-ar), then the aspect ratio is ignored.
{% endhint %}

{% tabs %}
{% tab title="Original" %}
URL - [https://ik.imagekit.io/ikmedia/sample-video.mp4](https://ik.imagekit.io/ikmedia/sample-video.mp4)
{% endtab %}

{% tab title="width 400px and aspect ratio 4:1" %}
URL - [https://ik.imagekit.io/ikmedia/sample-video.mp4?tr=ar-4-1,h-100](https://ik.imagekit.io/ikmedia/sample-video.mp4?tr=ar-4-1,h-100)
{% endtab %}
{% endtabs %}

## Crop, Crop Modes, and Focus

If only, one of the [height(h)](resize-crop-and-other-common-video-transformations.md#height-h) or [width(w)](resize-crop-and-other-common-video-transformations.md#width-w) is specified, then ImageKit.io adjusts the other dimension accordingly to preserve aspect ratio and no cropping takes place.

But when you specify both [height(h)](resize-crop-and-other-common-video-transformations.md#height-h) and [width(w)](resize-crop-and-other-common-video-transformations.md#width-w) dimensions, you need to choose the right cropping strategy based on your website layout and desired output video.

{% hint style="info" %}
**Tip for choosing the right cropping strategy**

When choosing among different strategies for cropping, think in terms of your website layout and desired output video dimension.
{% endhint %}

* If you want to preserve the whole video content without any cropping and need the exact same dimensions (height and width) in the output as requested, choose either the [pad resize crop](resize-crop-and-other-common-video-transformations.md#pad-resize-crop-strategy-cm-pad\_resize) or [forced crop strategy](resize-crop-and-other-common-video-transformations.md#forced-crop-strategy-c-force).
* If you want to preserve the whole video content without any cropping, but it is okay if one or both the dimensions (height or width) in the output are adjusted to preserve the aspect ratio. Then choose either the [max-size cropping](resize-crop-and-other-common-video-transformations.md#max-size-cropping-strategy-c-at\_max) or [min-size cropping](resize-crop-and-other-common-video-transformations.md#min-size-cropping-strategy-c-at\_least) strategy.
* If you need the exact same dimensions (height and width) in the output video as requested but it's okay to crop the video to preserve the aspect ratio. Then choose either the [maintain ratio crop](resize-crop-and-other-common-video-transformations.md#maintain-ratio-crop-strategy-c-maintain\_ratio) or the [extract crop](resize-crop-and-other-common-video-transformations.md#extract-crop-strategy-cm-extract) strategy. You can combine the extract crop strategy with different [focus](resize-crop-and-other-common-video-transformations.md#focus-fo) values to get the desired result.

### Pad resize crop strategy - (cm-pad\_resize)

In the pad resize crop strategy, the output dimension (height and width) is the same as requested, no cropping takes place, and the aspect ratio is preserved. This is accomplished by adding padding around the video to get it to match the exact dimension as requested.

#### Example - All padding on one side

In the examples above, we saw that when the video is padded using the [pad resize crop strategy](resize-crop-and-other-common-video-transformations.md#pad-resize-crop-strategy-cm-pad\_resize), the padding is equal on both sides of the video. However, there might be cases where we want all the padding to be added on only one side of the video. This can be done using the [focus (fo)](resize-crop-and-other-common-video-transformations.md#focus-fo) parameter.

{% tabs %}
{% tab title="Default crop (400x200)" %}
[https://ik.imagekit.io/demo/sample-video.mp4?tr=w-400,h-200](https://ik.imagekit.io/demo/sample-video.mp4?tr=w-400,h-200)\
\
The video is 400x200 but it is cropped from all sides to preserve the aspect ratio.

![](<../../.gitbook/assets/image (36).png>)
{% endtab %}

{% tab title="cm-pad_resize (center focus)" %}
[https://ik.imagekit.io/demo/sample-video.mp4?tr=w-400,h-200,cm-pad\_resize,bg-F3F3F3](https://ik.imagekit.io/demo/sample-video.mp4?tr=w-400,h-200,cm-pad\_resize,bg-F3F3F3)\
\
The video is exactly 400x200 and there is no cropping. Extra padding with [background color](resize-crop-and-other-common-video-transformations.md#background-bg) F3F3F3 has been added to get 400x200 output dimensions.

![](<../../.gitbook/assets/image (35).png>)
{% endtab %}

{% tab title="cm-pad_resize with blurred background" %}
[https://ik.imagekit.io/demo/sample2.mp4?tr=w-400,h-200,cm-pad\_resize,bg-blurred](https://ik.imagekit.io/demo/sample2.mp4?tr=w-400,h-200,cm-pad\_resize,bg-blurred)\
\
The video is exactly 700x700 and there is no cropping. The extra padding added consists of a [blurred background](resize-crop-and-other-common-video-transformations.md#background-bg) made using the source video to get 400x200 output dimensions.

![](../../.gitbook/assets/screenshot-cm-pad_resize,bg-blurred.png)
{% endtab %}

{% tab title="cm-pad_resize (left)" %}
You can also control the focus point using [fo parameter](resize-crop-and-other-common-video-transformations.md#focus-fo) to move the actual content to one side using relative positioning.

We added the `fo-left`. Now, all the padding is on the right of the video.

![](<../../.gitbook/assets/image (34).png>)
{% endtab %}
{% endtabs %}

### Forced crop strategy - (c-force)

In a forced crop strategy, the output video's dimension (height and width) is exactly the same as requested, no cropping takes place, but the aspect ratio is not preserved. It forcefully squeezes the original video to get it to fit completely within the output dimensions.&#x20;

URL - [https://ik.imagekit.io/demo/sample-video.mp4?tr=w-200,h-200,c-force](https://ik.imagekit.io/demo/sample-video.mp4?tr=w-200,h-200,c-force)\
\
Notice that the aspect ratio is changed and the video looks squeezed.

![](<../../.gitbook/assets/image (37).png>)



### Max-size cropping strategy - (c-at_max)

In the max-size crop strategy, whole video content is preserved without any cropping, the aspect ratio is preserved, but one of the dimensions (height or width) is adjusted.

The output video is less than or equal to the dimensions specified in the URL,i.e., at least one dimension will exactly match the output dimension requested, and the other dimension will be equal to or smaller than the corresponding output dimension requested.

URL - [https://ik.imagekit.io/demo/sample-video.mp4?tr=w-200,h-200,c-at\_max](https://ik.imagekit.io/demo/sample-video.mp4?tr=w-200,h-200,c-at\_max)

Notice that the aspect ratio is maintained and there is no cropping. But the height is reduced so that the video fits within a 200x200 container.

![](<../../.gitbook/assets/image (38).png>)

### Min-size cropping strategy - (c-at_least)

This strategy is similar to the [max-size cropping](resize-crop-and-other-common-video-transformations.md#max-size-cropping-strategy-c-at\_max) strategy, with the only difference being that, unlike the max-size strategy, the output video dimension is equal to or larger than the requested dimensions. One of the dimensions will be exactly the same as what is requested, while the other dimension will be equal to or larger than what is requested.

URL - [https://ik.imagekit.io/demo/sample-video.mp4?tr=w-200,h-200,c-at\_least](https://ik.imagekit.io/demo/sample-video.mp4?tr=w-200,h-200,c-at\_least)

Notice that the height is 200px as requested, but the width is more than 200px. The aspect ratio is maintained and there is no cropping.\


![](<../../.gitbook/assets/image (39).png>)

### Maintain ratio crop strategy - (c-maintain_ratio)

This is the default crop strategy. If no crop (`c`) or crop_mode (`cm`) is specified in the URL, `c-maintain_ratio` gets applied automatically.

In this strategy, the output video's dimensions (height and width) are the same as requested, and the aspect ratio is preserved. This is accomplished by resizing the video to the requested dimension and then cropping extra parts to get desired height & width.

{% hint style="info" %}
By default, ImageKit.io crops the video from the center but you can change this using the [focus parameter](../image-transformations/resize-crop-and-other-transformations.md#focus-fo).
{% endhint %}

{% tabs %}
{% tab title="Original" %}
URL - [https://ik.imagekit.io/demo/sample-video.mp4?tr=orig-true](https://ik.imagekit.io/demo/sample-video.mp4?tr=orig-true)

![Original video](../../.gitbook/assets/screenshot-sample-video-orig.png)
{% endtab %}

{% tab title="w-400,h-300,c-maintain_ratio" %}
URL - [https://ik.imagekit.io/demo/sample-video.mp4?tr=w-400,h-300,c-maintain_ratio](https://ik.imagekit.io/demo/sample-video.mp4?tr=w-400,h-300,c-maintain_ratio)

[`w-400,h-300,c-maintain_ratio`](https://ik.imagekit.io/demo/sample-video.mp4?tr=w-400,h-300,c-maintain_ratio) is equivalent to [`w-400,h-300`](https://ik.imagekit.io/demo/sample-video.mp4?tr=w-400,h-300) because `maintain_ratio` is the default crop strategy.

Notice that the video's dimension matches 400x300, but the content is cropped from all edges, i.e., by default, ImageKit will extract the video from the center.

![w-400,h-300,c-maintain_ratio](../../.gitbook/assets/screenshot-c-maintain_ratio-fo-center.png)
{% endtab %}

{% tab title="w-400,h-300,c-maintain_ratio,fo-left" %}
URL - [https://ik.imagekit.io/demo/sample-video.mp4?tr=w-400,h-300,c-maintain_ratio,fo-left](https://ik.imagekit.io/demo/sample-video.mp4?tr=w-400,h-300,c-maintain_ratio,fo-left)

Using the [focus (`fo`) parameter](resize-crop-and-other-common-video-transformations.md#focus-fo), you can specify the side to crop from.

Here the video's dimension matches 400x300 and crops out the content on right side, because focus (`fo`) is set to `left`.

![w-400,h-300,c-maintain_ratio,fo-left](../../.gitbook/assets/screenshot-c-maintain_ratio-fo-left.png)
{% endtab %}
{% endtabs %}

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

![](<../../.gitbook/assets/image (47).png>)
{% endtab %}

{% tab title="Relative focus" %}
In the relative method, you can use the [focus (fo) parameter](resize-crop-and-other-common-video-transformations.md#focus-fo) to specify that the extract should be done from, let's say, the bottom of the original video.

Valid relative values for `fo` parameters are - `center`, `top`, `left`, `bottom`, `right`, `top_left`, `top_right`, `bottom_left` and `bottom_right`.

![](<../../.gitbook/assets/image (49).png>)


{% endtab %}
{% endtabs %}

### Focus - (fo)

This parameter can be used along with [pad resize](resize-crop-and-other-common-video-transformations.md#pad-resize-crop-strategy-cm-pad\_resize), [maintain ratio](resize-crop-and-other-common-video-transformations.md#maintain-ratio-crop-strategy) or [extract crop](resize-crop-and-other-common-video-transformations.md#extract-crop-strategy-cm-extract) to change the behaviour of padding or cropping. Learn more from the different examples shown in respective sections.&#x20;

This parameter can have the following values depending upon where it is being used:

1. `left`, `right`, `top`, `bottom` can be to control the position of padding when used with pad resize. [Learn from example](resize-crop-and-other-common-video-transformations.md#example-all-padding-on-one-side).
2. `center`, `top`, `left`, `bottom`, `right`, `top_left`, `top_right`, `bottom_left` and `bottom_right` can be used to define relative cropping during extract crop. [Learn from examples.](resize-crop-and-other-common-video-transformations.md#examples-center-and-relative-focus)

## Commonly used transformations

### Quality - (q)

ImageKit.io allows you to choose a quality level between `1` and `100`. `1` results in the lowest perceptual quality and smallest file size. `100` results in the highest perceptual quality and biggest file size.

{% hint style="info" %}
Choosing 100 quality will not improve an already low-quality video. However, it will increase the size of the video file. It is recommended to not use a 100 quality setting.
{% endhint %}

**Default Value** - `50`. You can change default quality setting from the dashboard.

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

Possible values include `auto` ,`mp4` , `webm`.

**Default Value** - `auto`. You can disable [automatic video format conversion](../video-optimization/automatic-video-format-conversion.md) from the dashboard settings.&#x20;

### Audio codec - (ac)

Used to specify the audio codec for encoding the output.

Possible values include `aac`, `opus`, and `none`.

Use `none` for removing audio from the source video. So the output will have no audio.

### Video codec - (vc)

Used to specify the video codec for encoding the output.

Possible values include `h264`, `vp9`, `av1` and `none`

Use `none` for extracting audio from the source video. So the output will be an audio file.

#### Valid combinations for format (f), audio codec (ac) & video codec (vc)

| f    | ac   | vc   |
| ---- | ---- | ---- |
| mp4  | aac  | h264 |
| mp4  | aac  | none |
| mp4  | none | h264 |
| webm | opus | vp9  |
| webm | opus | none |
| webm | none | vp9  |
| webm | opus | av1  |
| webm | none | av1  |

### Named transformation - (n)

[Named Transformations](../named-transformations.md) are an alias for the entire transformation string. \
\
For example, we can create a named transformation - `video_thumbnail` for a transformation string - `tr:w-300,h-300` and is used like:

URL - [https://ik.imagekit.io/demo/sample-video.mp4?tr=n-video\_thumbnail](https://ik.imagekit.io/demo/sample-video.mp4?tr=n-video\_thumbnail)

![](<../../.gitbook/assets/image (50).png>)

### Background - (bg)

It is used to specify the background when using [crop-mode pad\_resize](resize-crop-and-other-common-video-transformations.md#pad-resize-crop-strategy-cm-pad\_resize) or border-radius (r). The background can be any solid colour or blurred frame of the video.

**Default Value** -  By default the background is black.

**Solid color**

* RGB Hex code: A 6-digit hex code (eg. AAFF00, 0f0fac)
* RGBA Hex code: An 8-digit hex code. Last two characters must be a number between `00` and `99`, specifying the opacity level (eg. AAFF0040, 0f0fac75)
* Color name: A standard color name in lowercase (eg. lightgreen, beige)

{% tabs %}
{% tab title="bg=red" %}
URL - [https://ik.imagekit.io/demo/tr:h-700,w-700,cm-pad\_resize,bg-red/sample2.mp4](https://ik.imagekit.io/demo/tr:h-700,w-700,cm-pad\_resize,bg-red/sample2.mp4)

![cm-pad\_resize-bg-red](../../.gitbook/assets/screenshot-cm_pad\_resize-bg-red.png)
{% endtab %}

{% tab title="bg=A94D34" %}
URL - [https://ik.imagekit.io/demo/tr:h-700,w-700,cm-pad\_resize,bg-A94D34/sample2.mp4](https://ik.imagekit.io/demo/tr:h-700,w-700,cm-pad\_resize,bg-A94D34/sample2.mp4)

![cm-pad\_resize-bg-A94D34](../../.gitbook/assets/screenshot-cm-pad\_resize-bg-A94D34.png)
{% endtab %}
{% endtabs %}

**Blurred background**

Syntax: `blurred[_<BlurIntensity>][_<Brightness>]`

|      Parameter |                              Value |                                                                                Description | Default |
| -------------: | ---------------------------------: | -----------------------------------------------------------------------------------------: | ------: |
| Blur intensity | `auto` or an integer from 0 to 100 |                                                                  Set the intensity of blur |  'auto' |
|     Brightness |           Integer from N255 to 255 | Negative value to make background darker and positive value to make the background lighter |       0 |

Limitation: Blurred background can only be used with `cm-pad_resize`

Blur intensity `auto`: The intensity of blur is automatically adjusted based on the provided height (h) & width (w) in the transformation.

{% tabs %}
{% tab title="bg=blurred" %}
URL - [https://ik.imagekit.io/demo/tr:h-700,w-700,cm-pad\_resize,bg-blurred/sample2.mp4](https://ik.imagekit.io/demo/tr:h-700,w-700,cm-pad\_resize,bg-blurred/sample2.mp4)

![cm-pad\_resize-bg-blurred](../../.gitbook/assets/screenshot-cm-pad\_resize,bg-blurred.png)
{% endtab %}

{% tab title="bg=blurred_25_N55" %}
URL - [https://ik.imagekit.io/demo/tr:h-700,w-700,cm-pad\_resize,bg-blurred\_25\_N55/sample2.mp4](https://ik.imagekit.io/demo/tr:h-700,w-700,cm-pad\_resize,bg-blurred\_25\_N55/sample2.mp4)

Customize blur-intensity to '25' & brightness to 'N55' (Make background darker)

![cm-pad\_resize-bg-blurred\_25\_N55](../../.gitbook/assets/screenshot-cm-pad\_resize,bg-blurred\_25\_N55.png)
{% endtab %}

{% tab title="bg=blurred_auto_30" %}
URL - [https://ik.imagekit.io/demo/tr:h-700,w-700,cm-pad\_resize,bg-blurred\_auto\_30/sample2.mp4](https://ik.imagekit.io/demo/tr:h-700,w-700,cm-pad\_resize,bg-blurred\_auto\_30/sample2.mp4)

Automatic blur-intensity & customize brightness to '30' (Make background lighter)

![cm-pad\_resize-bg-blurred\_auto\_30](../../.gitbook/assets/screenshot-cm-pad\_resize,bg-blurred\_auto\_30.png)
{% endtab %}
{% endtabs %}

### Border - (b)

This adds a border to the video. It accepts two parameters - the width of the border and the color of the border.

Usage - `b-<border-width>_<hex code>`

The width is specified as a number which is equivalent to the border width in pixels. The color code is specified as a 6-character hex code RRGGBB.

You can also provide an arithmetic expression such as `ih_div_20_red`, or `cw_mul_0.05_FF00FF` as a value for this parameter. Doing so will first calculate the value of the width and then apply transformations using that value. Learn more about arithmetic expressions [here](../arithmetic-expressions-in-transformations.md).

{% tabs %}
{% tab title="5px yellow border" %}
URL - [https://ik.imagekit.io/ikmedia/sample-video.mp4?tr=b-5_yellow](https://ik.imagekit.io/ikmedia/sample-video.mp4?tr=b-5_yellow)
{% endtab %}

{% tab title="15px red border" %}
URL - [https://ik.imagekit.io/ikmedia/sample-video.mp4?tr=b-15_red](https://ik.imagekit.io/ikmedia/sample-video.mp4?tr=b-15_red)
{% endtab %}

{% tab title="15px #5587d9" %}
URL - [https://ik.imagekit.io/ikmedia/sample-video.mp4?tr=b-15_5587d9](https://ik.imagekit.io/ikmedia/sample-video.mp4?tr=b-15_5587d9)
{% endtab %}

{% endtabs %}

### Radius - (r)

Get a video with a rounded corner. Control the radius of the corner using this parameter. To get a circle or oval shape, set the value to `max`.

Usage - `r-<value>`

**Possible Values** - Any positive integer or `max`.

{% hint style="info" %}
You can also change the background color of the video from the default black color using the [background parameter](resize-crop-and-other-common-video-transformations.md#background-bg).
{% endhint %}

{% tabs %}
{% tab title="r=20" %}
URL - [https://ik.imagekit.io/ikmedia/sample-video.mp4?tr=r-20](https://ik.imagekit.io/ikmedia/sample-video.mp4?tr=r-20)
{% endtab %}

{% tab title="r=50" %}
URL - [https://ik.imagekit.io/ikmedia/sample-video.mp4?tr=r-50](https://ik.imagekit.io/ikmedia/sample-video.mp4?tr=r-50)
{% endtab %}

{% tab title="r=max" %}
If the input asset has 1:1 aspect ratio, using `max` will result in a circle shape. However, if the input asset is rectangular, using `max` will result in an oval shape.

URL - [https://ik.imagekit.io/ikmedia/sample-video.mp4?tr=r-max](https://ik.imagekit.io/ikmedia/sample-video.mp4?tr=r-max)
{% endtab %}
{% endtabs %}

### Rotate - (rt)

It is used to specify the degree by which the output video must be rotated.

**Possible Values** - `0` , `90` , `180` , `270` and `360`.

{% tabs %}
{% tab title="rt=90" %}
URL - [https://ik.imagekit.io/ikmedia/sample-video.mp4?tr=w-200,rt-90](https://ik.imagekit.io/ikmedia/sample-video.mp4?tr=w-200,rt-90)
{% endtab %}

{% tab title="rt=180" %}
URL - [https://ik.imagekit.io/ikmedia/sample-video.mp4?tr=w-200,rt-180](https://ik.imagekit.io/ikmedia/sample-video.mp4?tr=w-200,rt-180)
{% endtab %}
{% endtabs %}

## Trimming

You can trim a video using using `so`, `eo` and `du` options to specify start offset, end offset or duration.

### Start offset - (so)

Specify start offset in seconds. The video before the start offset is removed from the output.

Usage - `so-<value>`

**Possible Values** - Specify the time in seconds as a positive number up to 2 decimal places e.g. 20 or 10.55. It must be less than the duration of the input video.

### End offset - (eo)

Specify end offset in seconds. The video after end offset is removed from the output.

Usage - `eo-<value>`

**Possible Values** - Specify the time in seconds as a positive number up to 2 decimal places e.g. 20 or 10.55. It must be less than the duration of the input video.

### Duration - (du)

Specify duration in seconds. It is often used with `so` to control duration of the video to keep in output starting after `so` seconds.

Usage - `du-<value>`

**Possible Values** - Specify the duration in seconds as a positive number up to 2 decimal places e.g. 20 or 10.55. It must be less than or equal to the duration of the input video.

## Get thumbnail from a video

To get the first frame from the video `ik-thumbnail.jpg` after the video resource URL.

```markup
https://ik.imagekit.io/demo/sample-video.mp4/ik-thumbnail.jpg
```

To get the snapshot from a specific time point, use the `so` parameter.

You can transform the output image using following parameters.

| Parameter   | Description |
| ----------- | ----------- |
| `l` and other [layer positional parameters](../video-transformation/overlay.md#position-of-layer).           | [Layers](../video-transformation/overlay.md#layers) to overlay images and text.   |
| [w](../image-transformations/resize-crop-and-other-transformations.md#width-w)                                 | Specify the width. |
| [h](../image-transformations/resize-crop-and-other-transformations.md#height-h)                                | Specify the height. |
| [ar](../image-transformations/resize-crop-and-other-transformations.md#aspect-ratio-ar)                        | Aspect ratio of output, when either width or height is specified. |
| [c](../image-transformations/resize-crop-and-other-transformations.md#crop-crop-modes-and-focus)               | Cropping method. Accepts `force`, `at_max`, and `at_least`. |
| [cm](../image-transformations/resize-crop-and-other-transformations.md#crop-crop-modes-and-focus)              | Crop mode. Supports `extract` and `pad_resize`. |
| [fo](../image-transformations/resize-crop-and-other-transformations.md#focus-fo)                               | Relative focus area used during cropping. Accpets `center`, `top`, `left`, `bottom`, `right`, `top_left`, `top_right`, `bottom_left` and `bottom_right`. |
| [b](../image-transformations/resize-crop-and-other-transformations.md#border-b)                                | This adds a border to the image. It accepts two parameters - the width of the border and the color of the border in format `b-<border-width>-<hex code>` |
| [bg](../image-transformations/resize-crop-and-other-transformations.md#background-bg)                    | It is used to specify the background that can be used along with some cropping strategies while resizing or applying border-radius (`r`). You can choose either a solid color (using standard-browser-colors, RGB hex code, or RGBA hex code to specify color) or a blurred version of the image (using the syntax `blurred[_<BlurIntensity>][_<Brightness>]`). Example: `bg-red`, `bg-AAFF00`, `bg-AAFF0010`, `bg-blurred`, `bg-blurred_25_N15`. |
| [r](../image-transformations/resize-crop-and-other-transformations.md#radius-r)                                | It is used to control the radius of the corner. To get a circle or oval shape, set the value to `max`. (Background color can be specified with `bg` parameter) |
| [so](../video-transformation/resize-crop-and-other-common-video-transformations.md#start-offset-so)         | Start offset of the video in unit of seconds to pick thumbnail from a specific time point. |

You can use [image-layer (`l-image`)](../video-transformation/overlay.md#transformation-of-image-overlay) to add overlay image and [text-layer (`l-text`)](../video-transformation/overlay.md#add-text-over-video) to add overlay text on the thumbnail. The use of layers in video-thumbnail & video-transformation is identical, therefore you can use time/duration based parameter (lso, leo & ldu) in the layer.

## Gif to MP4

To covert a gif to mp4 video, add `ik-gif-video.mp4` after the gif resource URL.

```markup
https://ik.imagekit.io/demo/gif-test.gif/ik-gif-video.mp4
```

You can transform the final video using any video transformation supported by ImageKit.
