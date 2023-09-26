---
description: >-
  A comprehensive guide that covers all the commonly used image transformations
  that you will need for your web applications.
---

# Resize, crop and other common transformations

## Basic image resizing

### Width - (w)

Used to specify the width of the output image. Accepts integer value greater than 1. If a value between 0 and 1 is specified, it acts as a percentage width. Therefore, 0.1 means 10% of the original width, 0.4 means 40% of the original width, and so on.

You can also specify the `auto`_ _value for this parameter (w-auto). Doing so will instruct ImageKit to read the width value from the [Width Client Hint](https://imagekit.io/responsive-images/#width) request header. Learn more about client hints [here](../client-hints.md).

{% tabs %}
{% tab title="Original" %}
URL - [https://ik.imagekit.io/demo/medium\_cafe\_B1iTdD0C.jpg](https://ik.imagekit.io/demo/medium\_cafe\_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/medium\_cafe\_B1iTdD0C.jpg)
{% endtab %}

{% tab title="200px wide" %}
URL - [https://ik.imagekit.io/demo/tr:w-200/medium\_cafe\_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:w-200/medium\_cafe\_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:w-200/medium\_cafe\_B1iTdD0C.jpg)
{% endtab %}

{% tab title="40% width of original image" %}
URL - [https://ik.imagekit.io/demo/tr:w-0.4/medium\_cafe\_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:w-0.4/medium\_cafe\_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:w-0.4/medium\_cafe\_B1iTdD0C.jpg)
{% endtab %}
{% endtabs %}

### Height - (h)

Used to specify the height of the output image. Accepts integer value greater than 1. If a value between 0 and 1 is specified, the value acts as a percentage height. Therefore, 0.1 means 10% of the original height, 0.4 means 40% of the original height, and so on.

{% tabs %}
{% tab title="Original" %}
URL - [https://ik.imagekit.io/demo/medium\_cafe\_B1iTdD0C.jpg](https://ik.imagekit.io/demo/medium\_cafe\_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/medium\_cafe\_B1iTdD0C.jpg)
{% endtab %}

{% tab title="200px height" %}
URL - [https://ik.imagekit.io/demo/tr:h-200/medium\_cafe\_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:h-200/medium\_cafe\_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:h-200/medium\_cafe\_B1iTdD0C.jpg)
{% endtab %}

{% tab title="40% height of the original image" %}
URL - [https://ik.imagekit.io/demo/tr:h-0.4/medium\_cafe\_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:h-0.4/medium\_cafe\_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:h-0.4/medium\_cafe\_B1iTdD0C.jpg)
{% endtab %}
{% endtabs %}

### Aspect ratio - (ar)

Used to specify the aspect ratio of the output image or the ratio of width to height of the output image. This parameter must be used along with either the [height(h)](resize-crop-and-other-transformations.md#height-h) or [width(w)](resize-crop-and-other-transformations.md#width-w) parameter. The format for specifying this transformation is `ar-<width>-<height>`

{% hint style="info" %}
If you specify both [height(h)](resize-crop-and-other-transformations.md#height-h) and [width(w)](resize-crop-and-other-transformations.md#width-w) in the URL along with [aspect ratio(ar)](resize-crop-and-other-transformations.md#aspect-ration-ar), then the aspect ratio is ignored.
{% endhint %}

{% tabs %}
{% tab title="Original" %}
URL - [https://ik.imagekit.io/demo/medium\_cafe\_B1iTdD0C.jpg](https://ik.imagekit.io/demo/medium\_cafe\_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/medium\_cafe\_B1iTdD0C.jpg)


{% endtab %}

{% tab title="width 400px and aspect ratio 4:3" %}
URL - [https://ik.imagekit.io/demo/tr:ar-4-3,w-400/medium\_cafe\_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:ar-4-3,w-400/medium\_cafe\_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:ar-4-3,w-400/medium\_cafe\_B1iTdD0C.jpg)
{% endtab %}
{% endtabs %}

## Crop, Crop Modes and Focus

If only, one of the [height(h)](resize-crop-and-other-transformations.md#height-h) or [width(w)](resize-crop-and-other-transformations.md#width-w) dimensions is specified, then ImageKit.io adjusts the other dimension accordingly to preserve aspect ratio and no cropping takes place.

But when you specify both [height(h)](resize-crop-and-other-transformations.md#height-h) and [width(w)](resize-crop-and-other-transformations.md#width-w) dimensions, you need to choose the right cropping strategy based on your website layout and desired output image.

{% hint style="info" %}
**Tip for choosing the right cropping strategy**\
When choosing among different strategies for cropping, think in terms of your website layout and desired output image.
{% endhint %}

* If you want to preserve the whole image content (no cropping) and need the exact same dimensions (height and width) in the output image as requested, choose either of the [pad resize crop](resize-crop-and-other-transformations.md#pad-resize-crop-strategy-cm-pad\_resize) or [forced crop strategy](resize-crop-and-other-transformations.md#forced-crop-strategy-c-force).
* If you want to preserve the whole image content (no cropping), but it is okay if one or both the dimensions (height or width) in the output image are adjusted to preserve the aspect ratio. Then choose either of the [max-size cropping](resize-crop-and-other-transformations.md#max-size-cropping-strategy-c-at\_max) or [min-size cropping strategy](resize-crop-and-other-transformations.md#min-size-cropping-strategy-c-at\_least). You can also use [max-size-enlarge cropping strategy](resize-crop-and-other-transformations.md#max-size-enlarge-cropping-strategy-c-at\_max\_enlarge) if you want to allow enlarging of image in case requested dimensions are more than original image dimension.
* If you need the exact same dimensions (height and width) in the output image as requested but it's okay to crop the image to preserve the aspect ratio (or extract a region from the original image). Then choose either of the [maintain ratio crop](resize-crop-and-other-transformations.md#maintain-ratio-crop-strategy-c-maintain\_ratio) or [extract crop](resize-crop-and-other-transformations.md#extract-crop-strategy-cm-extract) or [pad extract crop strategy](resize-crop-and-other-transformations.md#pad-extract-crop-strategy-cm-pad\_extract). You can combine the extract crop strategy with different [focus values](resize-crop-and-other-transformations.md#focus-fo) to get the desired result.

### Pad resize crop strategy - (cm-pad\_resize)

In the pad resize crop strategy, the output image's dimension (height and width) is the same as requested, no cropping takes place, and the aspect ratio is preserved. This is accomplished by adding padding around the output image to get it to match the exact dimension as requested.

{% tabs %}
{% tab title="Original" %}
URL - [https://ik.imagekit.io/demo/img/plant.jpeg](https://ik.imagekit.io/demo/img/plant.jpeg)

![Original 640x865 image](https://ik.imagekit.io/demo/img/plant.jpeg)
{% endtab %}

{% tab title="cm-pad_resize" %}
URL - [https://ik.imagekit.io/demo/img/plant.jpeg?tr=w-300,h-200,cm-pad\_resize,bg-F3F3F3](https://ik.imagekit.io/demo/img/plant.jpeg?tr=w-300,h-200,cm-pad\_resize,bg-F3F3F3)\
\
The output image is exactly 300x200. However, to maintain the aspect ratio and prevent cropping a solid colored padding is added around the resized image.

![300x200 image](https://ik.imagekit.io/demo/img/plant.jpeg?tr=w-300,h-200,cm-pad\_resize,bg-F3F3F3)

For sake of clarity, we have made the padding background slightly grey in color (#F3F3F3) using the background parameter (`bg`) in the URL.
{% endtab %}
{% endtabs %}

#### Example - All padding on one side

In the examples above, we saw that when the image is padded using the [pad resize crop strategy](resize-crop-and-other-transformations.md#pad-resize-crop-strategy-cm-pad\_resize), the padding is equal on both sides of the image. However, there might be cases where we want all the padding to be added on only one side of the image. This can be done using the [focus (fo)](resize-crop-and-other-transformations.md#focus-fo) parameter.

{% tabs %}
{% tab title="Padding to the right" %}
URL - [https://ik.imagekit.io/demo/img/plant.jpeg?tr=w-300,h-200,cm-pad\_resize,bg-F3F3F3,fo-left](https://ik.imagekit.io/demo/img/plant.jpeg?tr=w-300,h-200,cm-pad\_resize,bg-F3F3F3,fo-left)\
\
We added the `fo-left` transformation to our image. Now, all the padding is on the right of the image, while the image itself is on the left (determined by the value of [focus(fo)](resize-crop-and-other-transformations.md#focus-fo) parameter).

![](https://ik.imagekit.io/demo/img/plant.jpeg?tr=w-300,h-200,cm-pad\_resize,bg-F3F3F3,fo-left)
{% endtab %}

{% tab title="Padding to the bottom" %}
URL - [https://ik.imagekit.io/demo/img/plant.jpeg?tr=w-100,h-200,cm-pad\_resize,bg-F3F3F3,fo-top](https://ik.imagekit.io/demo/img/plant.jpeg?tr=w-100,h-200,cm-pad\_resize,bg-F3F3F3,fo-top)

We added the `fo-top` transformation to our image. Now, all the padding is on the bottom of the image, while the image itself is on the top (determined by the value of [focus(fo)](resize-crop-and-other-transformations.md#focus-fo) parameter).

![](https://ik.imagekit.io/demo/img/plant.jpeg?tr=w-100,h-200,cm-pad\_resize,bg-F3F3F3,fo-top)
{% endtab %}
{% endtabs %}

### Forced crop strategy - (c-force)

In a forced crop strategy, the output image's dimension (height and width) is exactly the same as requested, no cropping takes place, but the aspect ratio is not preserved. It forcefully squeezes the original image to get it to fit completely within the output dimensions.&#x20;

{% tabs %}
{% tab title="Original" %}
URL - [https://ik.imagekit.io/demo/img/plant.jpeg](https://ik.imagekit.io/demo/img/plant.jpeg)

![Original 640x865 image](https://ik.imagekit.io/demo/img/plant.jpeg)
{% endtab %}

{% tab title="c-force" %}
URL - [https://ik.imagekit.io/demo/img/plant.jpeg?tr=w-300,h-200,c-force](https://ik.imagekit.io/demo/img/plant.jpeg?tr=w-300,h-200,c-force)

![300x200 image](https://ik.imagekit.io/demo/img/plant.jpeg?tr=w-300,h-200,c-force)

The entire original image is preserved in the output image as well, but the hand and the plant have been squeezed to fit into the output image.
{% endtab %}
{% endtabs %}

### Max-size cropping strategy - (c-at\_max)

In the max-size crop strategy, whole image content is preserved (no cropping), the aspect ratio is preserved, but one of the dimensions (height or width) is adjusted.

The output image is less than or equal to the dimensions specified in the URL,i.e., at least one dimension will exactly match the output dimension requested, and the other dimension will be equal to or smaller than the corresponding output dimension requested.

If the requested dimension is more than the original dimension of the image, then the original image is returned without cropping. For enlarging image more than original dimensions check [max-size-enlarge cropping strategy](resize-crop-and-other-transformations.md#max-size-enlarge-cropping-strategy-c-at\_max_enlarge).

{% tabs %}
{% tab title="Original" %}
URL - [https://ik.imagekit.io/demo/img/plant.jpeg](https://ik.imagekit.io/demo/img/plant.jpeg)

![Original 640x865 image](https://ik.imagekit.io/demo/img/plant.jpeg)
{% endtab %}

{% tab title="c-at_max" %}
URL - [https://ik.imagekit.io/demo/img/plant.jpeg?tr=w-300,h-200,c-at\_max](https://ik.imagekit.io/demo/img/plant.jpeg?tr=w-300,h-200,c-at\_max)

![148x200 image](https://ik.imagekit.io/demo/img/plant.jpeg?tr=w-300,h-200,c-at\_max)

The entire image content and the aspect ratio is preserved. The output image dimensions are 148x200. So the height is exactly what is requested, but the width is smaller than what was requested.&#x20;

This mode is particularly useful if you have a container and you want to ensure that the image will never be larger than that container.
{% endtab %}
{% endtabs %}

### Max-size-enlarge cropping strategy - (c-at\_max_enlarge)

This strategy is similar to the [max-size cropping strategy](resize-crop-and-other-transformations.md#max-size-cropping-strategy-c-at\_max) with the addition that it also allows image to enlarge more than its original dimensions.

The output image is less than or equal to the dimensions specified in the URL,i.e., at least one dimension will exactly match the output dimension requested, and the other dimension will be equal to or smaller than the corresponding output dimension requested.

{% tabs %}
{% tab title="Original" %}
URL - [https://ik.imagekit.io/demo/img/plant.jpeg](https://ik.imagekit.io/demo/img/plant.jpeg)

![Original 640x865 image](https://ik.imagekit.io/demo/img/plant.jpeg)
{% endtab %}

{% tab title="c-at_max_enlarge" %}
URL - [https://ik.imagekit.io/demo/img/plant.jpeg?tr=w-850,h-1000,c-at\_max\_enlarge](https://ik.imagekit.io/demo/img/plant.jpeg?tr=w-850,h-1000,c-at\_max\_enlarge)

![740x1000 image](https://ik.imagekit.io/demo/img/plant.jpeg?tr=w-850,h-1000,c-at\_max\_enlarge)

The entire image content and the aspect ratio is preserved. The output image dimensions are 740x1000. So the height is exactly what is requested, but the width is smaller than what was requested.&#x20;

{% endtab %}
{% endtabs %}

### Min-size cropping strategy - (c-at\_least)

This strategy is similar to the [max-size cropping strategy](resize-crop-and-other-transformations.md#max-size-cropping-strategy-c-at\_max), with the only difference being that unlike the max-size strategy, the image is equal to or larger than the requested dimensions. One of the dimensions will be exactly the same as what is requested, while the other dimension will be equal to or larger than what is requested.

{% tabs %}
{% tab title="Original" %}
URL - [https://ik.imagekit.io/demo/img/plant.jpeg](https://ik.imagekit.io/demo/img/plant.jpeg)

![Original 640x865 image](https://ik.imagekit.io/demo/img/plant.jpeg)
{% endtab %}

{% tab title="c-at_least" %}
URL - [https://ik.imagekit.io/demo/img/plant.jpeg?tr=w-300,h-200,c-at\_least](https://ik.imagekit.io/demo/img/plant.jpeg?tr=w-300,h-200,c-at\_least)

![300x405 image](https://ik.imagekit.io/demo/img/plant.jpeg?tr=w-300,h-200,c-at\_least)

The entire image content and aspect ratio is preserved. The output image dimensions are 300x405. So the width is exactly the same as what was requested, but the height is larger than what was requested.

This is useful for cases where you want to have an image that is always at least as large as the container.
{% endtab %}
{% endtabs %}

### Maintain ratio crop strategy - (c-maintain\_ratio)

This is the default crop strategy. If nothing is specified in the URL, this strategy gets applied automatically. In this strategy, the output image's dimension (height and width) is the same as requested, and the aspect ratio is preserved. This is accomplished resizing the image to the requested dimension and in the process cropping parts from the original image.

{% hint style="info" %}
By default ImageKit.io extract the image from the center but you can change this using [focus parameter](resize-crop-and-other-transformations.md#focus-fo).
{% endhint %}

{% tabs %}
{% tab title="Original" %}
URL - [https://ik.imagekit.io/demo/img/plant.jpeg](https://ik.imagekit.io/demo/img/plant.jpeg)

![Original 640x865 image](https://ik.imagekit.io/demo/img/plant.jpeg)
{% endtab %}

{% tab title="c-maintain_ratio (center crop)" %}
URL - [https://ik.imagekit.io/demo/img/plant.jpeg?tr=w-300,h-200,c-maintain\_ratio](https://ik.imagekit.io/demo/img/plant.jpeg?tr=w-300,h-200,c-maintain\_ratio) or [https://ik.imagekit.io/demo/img/plant.jpeg?tr=w-300,h-200](https://ik.imagekit.io/demo/img/plant.jpeg?tr=w-300,h-200)

![300x200 cropped image](https://ik.imagekit.io/demo/img/plant.jpeg?tr=w-300,h-200,c-maintain\_ratio)

In the above image, the top and the bottom of the image got cropped out. But the aspect ratio has been preserved from the original to the output image i.e. the hand and the plant are not skewed or forcefully resized to fit the output dimensions.
{% endtab %}

{% tab title="c-maintain_ratio with fo-custom" %}
Original image URL - [https://ik.imagekit.io/demo/img/bike-image.jpeg](https://ik.imagekit.io/demo/img/bike-image.jpeg)

![](https://ik.imagekit.io/demo/img/bike-image.jpeg)

Using c-maintain\_ratio with fo-custom - [https://ik.imagekit.io/demo/img/bike-image.jpeg?tr=w-500,h-100,fo-custom](https://ik.imagekit.io/demo/img/bike-image.jpeg?tr=w-500,h-100,fo-custom)

![](https://ik.imagekit.io/demo/img/bike-image.jpeg?tr=w-500,h-100,fo-custom)
{% endtab %}
{% endtabs %}

### Extract crop strategy - (cm-extract)

In this strategy, the output image's dimension (height and width) is exactly the same as requested, and the aspect ratio is preserved. In this strategy, instead of trying to resize the image as we did in [maintain ratio strategy](resize-crop-and-other-transformations.md#maintain-ratio-crop-strategy-c-maintain\_ratio), we extract out a region of the requested dimension from the original image.

{% hint style="info" %}
By default, ImageKit.io extracts the image from the center but you can change this using the [focus parameter](resize-crop-and-other-transformations.md#focus-fo).
{% endhint %}

#### Examples - Center and relative focus

{% tabs %}
{% tab title="Original" %}
URL - [https://ik.imagekit.io/demo/img/plant.jpeg](https://ik.imagekit.io/demo/img/plant.jpeg)

![Original 640x865 image](https://ik.imagekit.io/demo/img/plant.jpeg)
{% endtab %}

{% tab title="cm-extract (default center extract)" %}
URL - [https://ik.imagekit.io/demo/img/plant.jpeg?tr=w-200,h-200,cm-extract](https://ik.imagekit.io/demo/img/plant.jpeg?tr=w-200,h-200,cm-extract)

![200x200 image](https://ik.imagekit.io/demo/img/plant.jpeg?tr=w-200,h-200,cm-extract)

The size of the hand and the plant has not changed at all from the original image, which means that no resizing has taken place. Instead, we have been able to extract out a 200x200 area from the original image. This is the regular center extract.
{% endtab %}

{% tab title="Relative focus" %}
In the relative method, you can use the [focus (fo) parameter](resize-crop-and-other-transformations.md#focus-fo) to specify that the extract should be done from, let's say, the bottom-right of the original image.

Valid relative values for `fo` parameters are - `center`, `top`, `left`, `bottom`, `right`, `top_left`, `top_right`, `bottom_left` and `bottom_right`.

Example URL -  [https://ik.imagekit.io/demo/img/plant.jpeg?tr=w-300,h-300,cm-extract,fo-bottom\_right](https://ik.imagekit.io/demo/img/plant.jpeg?tr=w-300,h-300,cm-extract,fo-bottom\_right)\
\
This is the extract done with focus on bottom right of the original image.

![This is the extract done with focus on bottom right of the original image.](https://ik.imagekit.io/demo/img/plant.jpeg?tr=w-300,h-300,cm-extract,fo-bottom\_right)
{% endtab %}
{% endtabs %}

#### Examples - Focus using cropped image coordinates

{% tabs %}
{% tab title="Focus using X, Y coordinates" %}
In the coordinate method of focus, we use the parameters `x` and `y` to determine the position of the top-left corner from where the extract would begin.

Example URL - [https://ik.imagekit.io/demo/img/plant.jpeg?tr=w-300,h-300,cm-extract,x-100,y-300](https://ik.imagekit.io/demo/img/plant.jpeg?tr=w-300,h-300,cm-extract,x-100,y-300)\
\
The extract is made starting from (100,300) point on the original image

![The extract is made starting from (100,300) point on the original image](https://ik.imagekit.io/demo/img/plant.jpeg?tr=w-300,h-300,cm-extract,x-100,y-300)
{% endtab %}

{% tab title="Focus using center coordinates (XC, YC)" %}
In the center coordinate method of focus, we use the parameters `xc` and `yc` to determine the position of the center of the image which would be extracted.

Example URL - [https://ik.imagekit.io/demo/img/plant.jpeg?tr=w-300,h-300,cm-extract,xc-300,yc-500](https://ik.imagekit.io/demo/img/plant.jpeg?tr=w-300,h-300,cm-extract,xc-300,yc-500)

The extract is made centered on (300, 500) point on the original image.

![](https://ik.imagekit.io/demo/img/plant.jpeg?tr=w-300,h-300,cm-extract,xc-300,yc-500)

This crop mode is really useful when you want to crop the image on the basis of the focal point of your resultant image.

An important point to note about the center coordinate crop mode is that if either the height or width dimension of the cropped image as per the given center coordinates goes beyond the bounds of the original image, the crop will fail.

So we suggest ensuring that you are using the correct height and width dimensions when cropping images using center coordinates.
{% endtab %}
{% endtabs %}

#### Example - **Focus using custom coordinates**

Instead of specifying the `x`, `y`, `xc` or `xy` coordinates to focus on a particular area, you can also specify the focus area in an image while uploading the image or from the media library and then use `fo-custom` transformation in the image URL. ImageKit will then utilize the custom crop area specified with the image for all crop operations. This custom focus mode works for both the [extract crop](resize-crop-and-other-transformations.md#extract-crop-strategy-cm-extract) and the default [maintain ratio crop](resize-crop-and-other-transformations.md#maintain-ratio-crop-strategy-c-maintain\_ratio) strategy.

{% tabs %}
{% tab title="Original" %}
URL - [https://ik.imagekit.io/demo/img/bike-image.jpeg](https://ik.imagekit.io/demo/img/bike-image.jpeg)

![](https://ik.imagekit.io/demo/img/bike-image.jpeg)
{% endtab %}

{% tab title="Focus using custom coordinates" %}
URL - [https://ik.imagekit.io/demo/img/bike-image.jpeg?tr=w-500,h-100,fo-custom,cm-extract](https://ik.imagekit.io/demo/img/bike-image.jpeg?tr=w-500,h-100,fo-custom,cm-extract)\
\
A 500x100px thumbnail with cm-extract crop strategy and fo-custom.

![](https://ik.imagekit.io/demo/img/bike-image.jpeg?tr=w-500,h-100,fo-custom,cm-extract)
{% endtab %}
{% endtabs %}

#### Unexpected behavior with auto rotation

ImageKit can be configured to [auto-rotate images](../image-optimization/metadata-color-profile-and-orientation.md#image-orientation) based on the `Orientation` value in the image metadata. This could result in unexpected behavior when using, `cm-extract`. In that case, you will have to adjust the values of `x` and `y` to accommodate for the oriented image.

### Pad extract crop strategy - (cm-pad_extract)

The pad extract crop strategy is an extension of the [extract crop strategy](resize-crop-and-other-transformations.md#extract-crop-strategy-cm-extract). In the extract crop strategy, we were extracting out a smaller area from a larger image. Now, there can be a scenario where the original image is small and we want to extract out a larger area (which is practically not possible without padding). So the pad extract mode adds a solid colored padding around the image to make it match the exact size requested.

This transformation is specified using the value `cm-pad_extract` transformation parameter in the URL.

{% tabs %}
{% tab title="Original" %}
URL - [https://ik.imagekit.io/demo/medium\_cafe\_B1iTdD0C.jpg](https://ik.imagekit.io/demo/medium\_cafe\_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/medium\_cafe\_B1iTdD0C.jpg)
{% endtab %}

{% tab title="cm-pad_extract" %}
URL - [https://ik.imagekit.io/demo/medium\_cafe\_B1iTdD0C.jpg?tr=w-700,h-700,cm-pad\_extract,bg-F3F3F3](https://ik.imagekit.io/demo/medium\_cafe\_B1iTdD0C.jpg?tr=w-700,h-700,cm-pad\_extract,bg-F3F3F3)\
\
Original image was 600x600 but this image is 700x700 image.

![](https://ik.imagekit.io/demo/medium\_cafe\_B1iTdD0C.jpg?tr=w-700,h-700,cm-pad\_extract,bg-F3F3F3)
{% endtab %}
{% endtabs %}

### Focus - (fo)

This parameter can be used along with [pad resize](resize-crop-and-other-transformations.md#pad-resize-crop-strategy-cm-pad\_resize), [maintain ratio](resize-crop-and-other-transformations.md#maintain-ratio-crop-strategy-c-maintain\_ratio) or [extract crop](resize-crop-and-other-transformations.md#extract-crop-strategy-cm-extract) to change the behaviour of padding or cropping. Learn more from the different examples shown in respective sections.&#x20;

This parameter can have following values depending upon where it is being used:

1. `left`, `right`, `top`, `bottom` can be to control the position of padding when used with pad resize. [Learn from example](resize-crop-and-other-transformations.md#example-all-padding-on-one-side).
2. `fo-custom` can be used to define a specific focus area when used with [maintain ratio](resize-crop-and-other-transformations.md#maintain-ratio-crop-strategy-c-maintain\_ratio) and [extract crop](resize-crop-and-other-transformations.md#example-focus-using-custom-coordinates).
3. `center`, `top`, `left`, `bottom`, `right`, `top_left`, `top_right`, `bottom_left` and `bottom_right` can be used to define relative cropping during extract crop. [Learn from examples](resize-crop-and-other-transformations.md#examples-center-and-relative-focus).

Apart from above, `fo` parameter also have two additional options that intelligently detect the most important part of an image to create thumbnails i.e. `auto` and `face`. Let's see them in action:

#### Auto smart cropping - (fo-auto)

In this mode, ImageKit automatically determines the most important part of the image and preserves it in the output thumbnail. This is enabled by passing the `fo-auto` parameter in the URL transformation parameters.

{% tabs %}
{% tab title="Original" %}
URL - [https://ik.imagekit.io/demo/img/girl.jpeg](https://ik.imagekit.io/demo/img/girl.jpeg)\
\
For example, in the image below, the girl is the main subject of the image and she is slightly towards the left from the center of the image.

![](https://ik.imagekit.io/demo/img/girl.jpeg)
{% endtab %}

{% tab title="Regular cropping" %}
URL - [https://ik.imagekit.io/demo/img/tr:w-200,h-300/girl.jpeg](https://ik.imagekit.io/demo/img/tr:w-200,h-300/girl.jpeg)\
\
If we use regular resize and the default crop strategy, we get the following result, where the main subject is off the center. This is definitely not a great thumb image to have.

![](https://ik.imagekit.io/demo/img/tr:w-200,h-300/girl.jpeg)
{% endtab %}

{% tab title="Smart crop (fo-auto)" %}
URL - [https://ik.imagekit.io/demo/img/tr:w-200,h-300,fo-auto/girl.jpeg](https://ik.imagekit.io/demo/img/tr:w-200,h-300,fo-auto/girl.jpeg)\
\
With smart crop auto mode, this is the how the final result looks like. The main subject is right in the center of the final thumbnail.

![](https://ik.imagekit.io/demo/img/tr:w-200,h-300,fo-auto/girl.jpeg)
{% endtab %}
{% endtabs %}

#### Face cropping - (fo-face)

In face crop, the crop works more like the [extract crop strategy](resize-crop-and-other-transformations.md#extract-crop-strategy-cm-extract), but instead of focusing on the center of the image, it finds out the face (or multiple faces) in the image and focuses around that. This gives you perfect thumbnails with just the subject's face that make up for good profile pictures.

This mode is enabled using 'fo-face' parameter in the URL transformation parameters.

{% tabs %}
{% tab title="Original" %}
URL - [https://ik.imagekit.io/demo/img/girl.jpeg](https://ik.imagekit.io/demo/img/girl.jpeg)

![](https://ik.imagekit.io/demo/img/girl.jpeg)
{% endtab %}

{% tab title="Smart crop (fo-face)" %}
URL - [https://ik.imagekit.io/demo/img/tr:w-150,h-150,fo-face/girl.jpeg](https://ik.imagekit.io/demo/img/tr:w-150,h-150,fo-face/girl.jpeg)\
\
Quite distinctly from the [auto smart crop](resize-crop-and-other-transformations.md#auto-smart-cropping-fo-auto), this thumbnail is focussed just around the face.

![](https://ik.imagekit.io/demo/img/tr:w-150,h-150,fo-face/girl.jpeg)
{% endtab %}
{% endtabs %}

{% hint style="info" %}
**Note: **Smart crop may not give accurate results for some images. This is partially a trade off between speed (needed for real-time transformations) and accuracy.
{% endhint %}

### Zoom - (z)

This parameter accepts a number that determines how much to zoom in or out of the cropped area. It must be used along with [fo-face](resize-crop-and-other-transformations.md#face-cropping-fo-face). A value less than 1.0 zooms out to include more background surrounding the face, whereas a value larger than 1.0 zooms in to exclude more background surrounding the face.

**Default Value** - `z = 1.0`

**Possible Values** -  `0 < z < 1` (zoom out); `z > 1` (zoom in)


{% tabs %}
{% tab title="Original" %}
URL - [https://ik.imagekit.io/demo/img/girl.jpeg](https://ik.imagekit.io/demo/img/girl.jpeg)

![](https://ik.imagekit.io/demo/img/girl.jpeg)
{% endtab %}

{% tab title="Smart crop (fo-face)" %}
URL - [https://ik.imagekit.io/demo/img/tr:w-150,h-150,fo-face/girl.jpeg](https://ik.imagekit.io/demo/img/tr:w-150,h-150,fo-face/girl.jpeg)

![](https://ik.imagekit.io/demo/img/tr:w-150,h-150,fo-face/girl.jpeg)
{% endtab %}

{% tab title="zoom=2.0" %}
URL - [https://ik.imagekit.io/demo/img/tr:w-150,h-150,fo-face,z-2.0/girl.jpeg](https://ik.imagekit.io/demo/img/tr:w-150,h-150,fo-face,z-2.0/girl.jpeg)\
\
For `z=2.0`, a thumbnail is generated zoomed in on the face with a zoom amount of 200%, excluding more background surrounding the face.

![](https://ik.imagekit.io/demo/img/tr:w-150,h-150,fo-face,z-2.0/girl.jpeg)
{% endtab %}

{% tab title="zoom=0.5" %}
URL - [https://ik.imagekit.io/demo/img/tr:w-150,h-150,fo-face,z-0.5/girl.jpeg](https://ik.imagekit.io/demo/img/tr:w-150,h-150,fo-face,z-0.5/girl.jpeg)\
\
For `z=0.5`, a thumbnail is generated zoomed out to 50% before cropping, including more of the background surrounding the face.

![](https://ik.imagekit.io/demo/img/tr:w-150,h-150,fo-face,z-0.5/girl.jpeg)
{% endtab %}
{% endtabs %}

## Commonly used transformations

### Quality - (q)

Used to specify the quality of the output image for lossy formats like JPEG, WebP and AVIF.  A large quality number indicates a larger output image size with high quality. A small quality number indicates a smaller output image size with lower quality. 

**Default Value** - `80` (can be managed from image settings in dashboard)

{% tabs %}
{% tab title="quality=90" %}
URL - [https://ik.imagekit.io/demo/tr:q-90/medium\_cafe\_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:q-90/medium\_cafe\_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:q-90/medium\_cafe\_B1iTdD0C.jpg)
{% endtab %}

{% tab title="quality=10" %}
URL - [https://ik.imagekit.io/demo/tr:q-10/medium\_cafe\_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:q-10/medium\_cafe\_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:q-10/medium\_cafe\_B1iTdD0C.jpg)
{% endtab %}
{% endtabs %}

### Format - (f)

Used to specify the format of the output image. If no output image format is specified then based on your image settings in the dashboard, ImageKit.io [automatically picks the best format](../image-optimization/automatic-image-format-conversion.md) for that image request.

&#x20;Possible values include `auto` ,`jpg` , `jpeg` , `webp`, `avif` and `png`

**Default Value** - `auto` (from dashboard settings)

{% hint style="info" %}
A few WebP images may not render correctly in Safari v14+ on MacOS v11+ and IOS 14 because of a [possible OS-level issue](https://bugs.webkit.org/show\_bug.cgi?id=219977).
{% endhint %}

### Blur - (bl)

Used to specify the gaussian blur that must be applied to an image. The value of `bl` specifies the radius of the Gaussian Blur that is to be applied. Higher the value, the larger the radius of Gaussian Blur. Possible values include integers between `1` and `100`.

{% tabs %}
{% tab title="Original" %}
URL - [https://ik.imagekit.io/demo/medium\_cafe\_B1iTdD0C.jpg](https://ik.imagekit.io/demo/medium\_cafe\_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/medium\_cafe\_B1iTdD0C.jpg)
{% endtab %}

{% tab title="blur=10" %}
URL - [https://ik.imagekit.io/demo/tr:bl-10/medium\_cafe\_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:bl-10/medium\_cafe\_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:bl-10/medium\_cafe\_B1iTdD0C.jpg)
{% endtab %}
{% endtabs %}

### Grayscale - (e-grayscale)

Used to turn an image to a grayscale version.

{% tabs %}
{% tab title="Normal" %}
URL - [https://ik.imagekit.io/demo/tr:h-300/sample\_image.jpg](https://ik.imagekit.io/demo/tr:h-300/sample\_image.jpg)

![](https://ik.imagekit.io/demo/tr:h-300/sample\_image.jpg)
{% endtab %}

{% tab title="Grayscale" %}
URL - [https://ik.imagekit.io/demo/tr:h-300,e-grayscale/sample\_image.jpg](https://ik.imagekit.io/demo/tr:h-300,e-grayscale/sample\_image.jpg)

![](https://ik.imagekit.io/demo/tr:h-300,e-grayscale/sample\_image.jpg)
{% endtab %}
{% endtabs %}

### DPR - (dpr)

Used to specify the device pixel ratio that is used to calculate the dimensions of the output image. Extremely helpful when creating image transformations for devices with a high device pixel ratio (DPR > 1), like the iPhone or high-end Android devices.

The `dpr` parameter can only be used when either the height or width of the desired output image is specified.

If the output image's height or width after considering the specified DPR is less than 1px or greater than 5000px, the value of DPR is not considered and the height or width used in the URL is used.

**Possible Values**- `0.1`  to `5` .

Alternatively, you can specify the `auto` value for this parameter (`dpr-auto`). Doing so will instruct ImageKit to read the `dpr` value from the [DPR Client Hint](https://imagekit.io/responsive-images/#dpr) request header. Learn more about client hints [here](../client-hints.md).

### Named transformation - (n)

[Named Transformations](../named-transformations.md) are an alias for the entire transformation string. \
\
For example, we can create a named transformation - `thumbnail` for a transformation string - `tr:w-100,h-100,c-at_max,fo-auto` and is used like:\
\
[https://ik.imagekit.io/demo/img/tr:n-media\_library\_thumbnail/default-image.jpg](https://ik.imagekit.io/demo/img/tr:n-media\_library\_thumbnail/default-image.jpg)

### Default image - (di)

When an image that is requested using Imagekit.io does not exist, a 404 error is displayed. In certain scenarios, you would want a default image to be delivered instead of the 404 error message.

Imagekit.io provides an alternative to achieve this without having to make changes at the application level using this parameter. The image specified in this parameter should be accessible using the default endpoint of your ImageKit.io account.\
\
[https://ik.imagekit.io/demo/tr:di-medium\_cafe\_B1iTdD0C.jpg/non\_existent\_image.jpg](https://ik.imagekit.io/demo/tr:di-medium\_cafe\_B1iTdD0C.jpg/non\_existent\_image.jpg)

> If the default image is nested inside multiple folders, then you need to specify the entire default image path in this parameter. Replace `/` with `@@` in the default image path. For example, if the default image is accessible on `https://ik.imagekit.io/demo/path/to/image.jpg`, then the `di` parameter should be `di-@@path@@to@@image.jpg`.



### Progressive image - (pr)

Used to specify whether the output JPEG image must be rendered progressively. In progressive loading, the output image renders as a low quality pixelated full image which over time keeps on adding more pixels and information to the image.  This helps you maintain a fast perceived load time. \
\
**Possible values **include `true`  and `false` .\
\
[https://ik.imagekit.io/demo/tr:h-300,w-400,f-jpg,pr-true/medium\_cafe\_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:h-300,w-400,f-jpg,pr-true/medium\_cafe\_B1iTdD0C.jpg)

### Lossless WebP and PNG - (lo)

Used to specify whether the output image (if in JPEG or PNG) must be compressed losslessly. In lossless compression, the output file size is larger than the regular lossy compression. However, the perceived image quality can be higher in certain cases, especially for computer-generated graphics. Using lossless compression for photographs is not recommended.\
\
**Possible Values**- `true`  and `false` \
\
[https://ik.imagekit.io/demo/tr:w-500,h-361,lo-true/medium\_cafe\_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:w-500,h-361,lo-true/medium\_cafe\_B1iTdD0C.jpg)

### Trim edges - (t)

Useful with images that have a solid or nearly solid background with the object in the center. This parameter trims the background from the image, leaving only the central object in the output image. 

Usage - `t-true|Number`\
\
**Possible Values** include `true`  and integer values between `1`  and `99` that specify the threshold level for considering a particular pixel as "background".

{% tabs %}
{% tab title="Trim true" %}
URL - [https://ik.imagekit.io/demo/img/tr:t-true/trim\_example\_BkgQVu7oX.png](https://ik.imagekit.io/demo/img/tr:t-true/trim\_example\_BkgQVu7oX.png)

![](https://ik.imagekit.io/demo/img/tr:t-true/trim\_example\_BkgQVu7oX.png)
{% endtab %}

{% tab title="With trim equal to 5" %}
URL - [https://ik.imagekit.io/demo/img/tr:t-5/trim\_example\_BkgQVu7oX.png](https://ik.imagekit.io/demo/img/tr:t-5/trim\_example\_BkgQVu7oX.png)\
\
Here some trimming does take place (you can notice that the grey circle in the background is getting cut off), but it is not as much as what happened when the value of trim was set to `true`

![](https://ik.imagekit.io/demo/img/tr:t-5/trim\_example\_BkgQVu7oX.png)
{% endtab %}

{% tab title="With trim equal to 15" %}
URL - [https://ik.imagekit.io/demo/img/tr:t-15/trim\_example\_BkgQVu7oX.png](https://ik.imagekit.io/demo/img/tr:t-15/trim\_example\_BkgQVu7oX.png)\
\
Here the amount of trimming is higher than what happened with trim set to `true`. This is because with a higher value specified for trim, more and more pixels are considered as similar and redundant and hence get removed.

![](https://ik.imagekit.io/demo/img/tr:t-15/trim\_example\_BkgQVu7oX.png)
{% endtab %}
{% endtabs %}

### Border - (b)

This adds a border to the image. It accepts two parameters - the width of the border and the color of the border.

Usage - `b-<border-width>-<hex code>`

The width is specified as a number which is equivalent to the border width in pixels. The color code is specified as a 6-character hex code RRGGBB.

{% tabs %}
{% tab title="5px yellow border" %}
URL - [https://ik.imagekit.io/demo/tr:w-300,b-5\_FFF000/medium\_cafe\_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:w-300,b-5\_FFF000/medium\_cafe\_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:w-300,b-5\_FFF000/medium\_cafe\_B1iTdD0C.jpg)
{% endtab %}

{% tab title="20px red border" %}
URL - [https://ik.imagekit.io/demo/tr:w-300,b-20\_FF0000/medium\_cafe\_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:w-300,b-20\_FF0000/medium\_cafe\_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:w-300,b-20\_FF0000/medium\_cafe\_B1iTdD0C.jpg)
{% endtab %}
{% endtabs %}

### Color profile - (cp)

It is used to specify whether the output image should contain the color profile that is initially available with the original image. It is recommended to remove the color profile before serving the images on web and apps.  However, if you feel that the output image looks faded or washed out after using ImageKit.io, and want to preserve the colors of your original image, you should set this parameter to `true`.

**Default Value**- `false` (from the dashboard)

**Possible Values**- `true`  and `false` 

{% tabs %}
{% tab title="cp=false (Default)" %}
URL - [https://ik.imagekit.io/demo/tr:h-300,w-400,f-jpg,cp-false/medium\_cafe\_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:h-300,w-400,f-jpg,cp-false/medium\_cafe\_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:h-300,w-400,f-jpg,cp-false/medium\_cafe\_B1iTdD0C.jpg)
{% endtab %}

{% tab title="cp=true" %}
URL - [https://ik.imagekit.io/demo/tr:h-300,w-400,f-jpg,cp-true/medium\_cafe\_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:h-300,w-400,f-jpg,cp-true/medium\_cafe\_B1iTdD0C.jpg)\
\
You might not observe any change in this particular image. It depends on if the original image has any specific color profile or not.

![](https://ik.imagekit.io/demo/tr:h-300,w-400,f-jpg,cp-true/medium\_cafe\_B1iTdD0C.jpg)
{% endtab %}
{% endtabs %}

### Image metadata - (md)

It is used to specify whether the output image should contain all the metadata that is initially available with the original image. Image metadata is not relevant for rendering on the web and apps. It is, thus, recommended to not use it while delivering images. The only situation to enable the metadata option is when you want additional data like camera information, lens information, and other image profiles attached to your original image. 

**Default Value**- `false`  (from the dashboard)

**Possible Values**- `true`  and `false` 

### Rotate - (rt)

It is used to specify the degree by which the output image must be rotated or specifies the use of EXIF Orientation Tag for the rotation of image using the `auto` parameter.

**Possible Values**- Any number for a clockwise rotation, or any number preceded with `N` for counter-clockwise rotation , and `auto`

{% hint style="info" %}
Use `auto` if you want ImageKit.io to automatically rotate image based on EXIF orientation tag in image metadata.
{% endhint %}

{% tabs %}
{% tab title="rt=90" %}
URL - [https://ik.imagekit.io/demo/tr:rt-90/medium\_cafe\_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:rt-90/medium\_cafe\_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:rt-90/medium\_cafe\_B1iTdD0C.jpg)
{% endtab %}

{% tab title="rt=N40" %}
URL - [https://ik.imagekit.io/demo/tr:rt-N40/medium\_cafe\_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:rt-N40/medium\_cafe\_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:rt-N40/medium\_cafe\_B1iTdD0C.jpg)
{% endtab %}
{% endtabs %}

### Radius - (r)

It is used to specify the radius that must be used to obtain a rounded output image. To obtain a perfectly rounded image, set the value to `max` . This parameter is applied after resizing the original image, if defined.

**Possible Values** - Positive Integer and `max`

{% hint style="info" %}
For simpler cases, you can use radius in the same transformation as the height and width parameters. However, if you are using advanced cropping parameters, like crop (c) and crop mode (cm), then you should [chain](chained-transformations.md) the radius transformation in a step after the resizing transformation.
{% endhint %}

{% tabs %}
{% tab title="r=20" %}
URL - [https://ik.imagekit.io/demo/tr:r-20/medium\_cafe\_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:r-20/medium\_cafe\_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:r-20/medium\_cafe\_B1iTdD0C.jpg)
{% endtab %}

{% tab title="r=50" %}
URL - [https://ik.imagekit.io/demo/tr:r-50/medium\_cafe\_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:r-50/medium\_cafe\_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:r-50/medium\_cafe\_B1iTdD0C.jpg)
{% endtab %}

{% tab title="r=max (Round)" %}
URL - [https://ik.imagekit.io/demo/tr:r-max/medium\_cafe\_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:r-max/medium\_cafe\_B1iTdD0C.jpg)\
\
This is good for creating profile pictures.

![](https://ik.imagekit.io/demo/tr:r-max/medium\_cafe\_B1iTdD0C.jpg)
{% endtab %}
{% endtabs %}

### Background color - (bg)

It is used to specify the background color that can be used along with some cropping strategies while resizing an image.

**Possible Values**
* RGB Hex code: A 6-digit hex code (eg. AAFF00, 0f0fac)
* RGBA Hex code: An 8-digit hex code. Last two characters must be a number between `00` and `99`, specifying the opacity level (eg. AAFF0040, 0f0fac75) 
* Color name: A standard color name in lowercase (eg. lightgreen, beige)

**Default Value** - black (00000)

{% tabs %}
{% tab title="bg=272B38" %}
URL - [https://ik.imagekit.io/demo/tr:h-700,w-700,cm-pad\_extract,bg-272B38/medium\_cafe\_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:h-700,w-700,cm-pad\_extract,bg-272B38/medium\_cafe\_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:h-700,w-700,cm-pad\_extract,bg-272B38/medium\_cafe\_B1iTdD0C.jpg)
{% endtab %}

{% tab title="bg=A94D34" %}
URL - [https://ik.imagekit.io/demo/tr:h-700,w-700,cm-pad\_extract,bg-A94D34/medium\_cafe\_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:h-700,w-700,cm-pad\_extract,bg-A94D34/medium\_cafe\_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:h-700,w-700,cm-pad\_extract,bg-A94D34/medium\_cafe\_B1iTdD0C.jpg)
{% endtab %}
{% endtabs %}

#### **Specifying a background for a PNG image**

URL - [https://ik.imagekit.io/demo/img/logo-white\_SJwqB4Nfe.png?tr=bg-F00000](https://ik.imagekit.io/demo/img/logo-white\_SJwqB4Nfe.png?tr=bg-F00000)

![](https://ik.imagekit.io/demo/img/logo-white\_SJwqB4Nfe.png?tr=bg-F00000)

### Original image - (orig)

By default, any image that is delivered via ImageKit.io always gets optimized in some way or the other. While this automatic optimization is great for web and apps, there might be certain cases where you want to get the original image as is. You can do so by using the parameter `orig`  and set the value to `true` .  If there are any other transformation parameters specified along with `orig-true` , then those get ignored.

Example - [https://ik.imagekit.io/demo/tr:orig-true/medium\_cafe\_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:orig-true/medium\_cafe\_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:orig-true/medium\_cafe\_B1iTdD0C.jpg)

### Downloading file - (ik-attachment)

Set `ik-attachment=true` query param in the URL to download the file as an attachment rather than viewing it inline in the web browser.

By specifying this parameter in the file URL, ImageKit sets the `content-disposition` header in the response with the value `attachment` along with the file name.

Example - [https://ik.imagekit.io/demo/medium_cafe_B1iTdD0C.jpg?ik-attachment=true](https://ik.imagekit.io/demo/medium_cafe_B1iTdD0C.jpg?ik-attachment=true)

![](https://ik.imagekit.io/demo/medium_cafe_B1iTdD0C.jpg?ik-attachment=true)
