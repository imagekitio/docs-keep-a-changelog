---
description: >-
  A comprehensive guide that covers how you can add images, text, subtitles, and videos over
  a base video.
---

# Overlay

In ImageKit, you can add images, text, subtitles, and videos over a base video using [layers](#layers). Skip to the relevant section to understand with a quick example:

* [Add images over video](overlay.md#add-images-over-video)
* [Add text over video](overlay.md#add-text-over-video)
* [Add video over video](overlay.md#add-video-over-video)
* [Add solid color blocks over video](overlay.md#add-solid-color-blocks-over-video)
* [Add subtitles over a video](overlay.md#add-subtitles-over-a-video)

# Layers
A layer is a special kind of transformation that allows you to modify the overlay itself and express its position in relation to the parent.

## Syntax of layers

A layer starts with `l-<type>` and ends with `l-end`. All the positional and transformation parameters of that layer are between `l-<type>` and `l-end` and only apply to that layer and not the parent base asset.

`type` can be `image`, `text`, `video` or `subtitles`.

For example, in the following URL, we are adding a logo image `logo.png` on top of a base video, i.e. `sample-video.mp4`. However, we applied width (`w-10`) and rotation (`rt-90`) transformations on this overlay logo image before placing it on the base video. Transformations `w-300,h-300` are applied to `sample-video.mp4`.

Here the parent base video has one layer inside it. A layer can also nest another layer.

```markup
        URL-endpoint           Base video                                   Layer
┌──────────────────────────┐┌──────────────┐                ┌─────────────────────────────────┐
https://ik.imagekit.io/demo/sample-video.mp4?tr=w-300,h-300,l-image,i-logo.png,w-10,rt-90,l-end
                                                └────┬────┘                    └────┬────┘
                                                     │                              │
                                                     │        These transformations are applied to logo.png
                                                     │
                           These transformations are applied to sample-video.mp4
```

## Input of layer
The input of the layer can be specified using `i` or `ie` (base64 encoded) parameter. In case both `i` and `ie` is present, `i` is ignored. The base64 string should be made URL safe to ensure that all padding characters(=) are included correctly. In Javascript, a function like `encodeURIComponent()` can be used for this.

## Position of layer

The position of the layer can be controlled using the following parameters. The position of the layer is always relative to the immediate parent. For instance, the parent is the base video in the above example, and the logo image is the nested layer. 

{% hint style="info" %}
The position of subtitles cannot be controlled at this point.
{% endhint %}

| Parameter   | Description |
| ----------- | ----------- |
| lx          | `x` of the top-left corner in the base asset where the layer's top-left corner would be placed. It can also accept arithmetic expressions such as `bw_mul_0.4`, or `bw_sub_cw`. Learn more about arithmetic expressions [here](../arithmetic-expressions-in-transformations.md).  |
| ly          | `y` of the top-left corner in the base asset where the layer's top-left corner would be placed.  It can also accept arithmetic expressions such as `bh_mul_0.4`, or `bh_sub_ch`. Learn more about arithmetic expressions [here](../arithmetic-expressions-in-transformations.md).  |
| lfo         | Position of the layer in relative terms e.g. `center`, `top`, `left`, `bottom`, `right`, `top_left`, `top_right`, `bottom_left` and `bottom_right`. Default value is `center`.      |
| lso         | Start time of the base video in seconds when the layer should appear. It accepts a positive number upto two decimal e.g. 20 or 20.50 |
| ldu         | Duration in seconds during which layer should appear on the base video. It accepts a positive number upto two decimal e.g. 20 or 20.50 |
| leo         | End time of the base video when this layer should disappear. In case both `leo` and `ldu` are present, `ldu` is ignored. It accepts a positive number upto two decimal e.g. 20 or 20.50 |

## Transformation of layer

You can apply transformations to the layer as you would otherwise. However, any transformation parameter inside the layer is only applicable to that layer and not the parent.

However, different types of layers support different types of transformations which are covered in respective sections.

* [Transformation of images overlay](overlay.md#transformation-of-image-overlay)
* [Transformation of text overlay](overlay.md#transformation-of-text-overlay)
* [Transformation of video overlay](overlay.md#transformation-of-video-overlay)
* [Transformation of solid color blocks overlay](overlay.md#transformation-of-solid-color-blocks-overlay)
* [Transformation of subtitles overlay](overlay.md#transformation-of-subtitles-overlay)

Transformations inside a layer can be chained together to achieve the desired outcome.

## Nesting of layers

A layer can have a nested layer up to 3 levels. 

For example, in the below URL, `i-inner.png` is rendered on the top-left corner of `i-outer.png` using the `lfo-top_left` parameter.

```markup
                          Parent layer
┌──────────────────────────────────────────────────────────────┐
l-image,i-outer.png,l-image,i-inner.png,lfo-top_left,l-end,l-end
                    └──────────────────┬──────────────────┘
                                       │
                                  Nested layer
```

# Add images over video

You can add an image over a base video using image layer (`l-image`).

## Usage

```markup
https://ik.imagekit.io/demo/sample-video.mp4?tr=l-image,i-logo.png,l-end
```

You can also control the position of image overlay using these [positional parameters](overlay.md#position-of-layer).

Input (`i` or `ie`) for `l-image` can be JPG, PNG, WEBP, TIFF or GIF files.

{% hint style="info" %}
When GIF file is used in image layer, the 1st frame of the GIF is used as a static image. For animated GIF you can use [video layer (`l-video`)](#add-video-over-video).

Animated-WebP and animated-PNG (`apng`) are not supported in Imagekit's video API.
{% endhint %}

## Transformation of image overlay

ImageKit supports many [image transformation parameters](../image-transformations/README.md), however inside a layer, you can apply the following transformations to the image overlay before it is placed on the base video. You can [chain the transformations](../image-transformations/chained-transformations.md) one after other to achieve the desired outcome.

| Parameter                                                                                                      | Description                  |
|----------------------------------------------------------------------------------------------------------------|------------------------------|
| [w](../image-transformations/resize-crop-and-other-transformations.md#width-w)                                 | Width of overlay image. It can also accept arithmetic expressions such as `bw_div_2`, or `bw_sub_cw`. Learn more about arithmetic expressions [here](../arithmetic-expressions-in-transformations.md).  |
| [h](../image-transformations/resize-crop-and-other-transformations.md#height-h)                                | Height of overlay image. It can also accept arithmetic expressions such as `bh_div_2`, or `bh_sub_cw`. Learn more about arithmetic expressions [here](../arithmetic-expressions-in-transformations.md). |
| [ar](../image-transformations/resize-crop-and-other-transformations.md#aspect-ratio-ar)                        | Aspect ratio of overlay image. It can also accept arithmetic expressions such as `iar_div_2`, or `bar`. Learn more about arithmetic expressions [here](../arithmetic-expressions-in-transformations.md). |
| [c](../image-transformations/resize-crop-and-other-transformations.md#crop-crop-modes-and-focus)               | Cropping method. Accepts `force`, `at_max`, and `at_least`. |
| [cm](../image-transformations/resize-crop-and-other-transformations.md#crop-crop-modes-and-focus)              | Crop mode. Supports `extract` and `pad_resize`. |
| [fo](../image-transformations/resize-crop-and-other-transformations.md#focus-fo)                               | Relative focus area used during cropping. Accpets `center`, `top`, `left`, `bottom`, `right`, `top_left`, `top_right`, `bottom_left` and `bottom_right`. |
| [b](../image-transformations/resize-crop-and-other-transformations.md#border-b)                                | This adds a border to the overlay image. It accepts two parameters - the width of the border and the color of the border in format `b-<border-width>-<hex code>`. It can also accept arithmetic expressions such as `ih_div_20_red`, or `cw_mul_0.05_FF0000`. Learn more about arithmetic expressions [here](../arithmetic-expressions-in-transformations.md). |
| [bg](../image-transformations/resize-crop-and-other-transformations.md#background-bg)                    | It is used to specify the background color in RGB Hex Code (e.g. FF0000) or an RGBA Code (e.g. FFAABB50) that must be used for the image. If you specify an 8 character background, the last two characters must be a number between 00 and 99 , which is used to indicate the opacity level of the background. `00` represents an opacity level of `0.00`, `01`  represents opacity level `0.01`, and so on. |
| [r](../image-transformations/resize-crop-and-other-transformations.md#radius-r)                                | It is used to control the radius of the corner. To get a circle or oval shape, set the value to `max`. |
| [rt](../video-transformations/resize-crop-and-other-common-video-transformations.md#rotate-rt)                                | It is used to specify the degree by which the overlay image must be rotated. |

# Add text over video

You can add any text string over a base video using the following example.

## Usage syntax

```markup
https://ik.imagekit.io/demo/sample-video.mp4?tr=l-text,i-hello,l-end
```

You can also control the position of text overlay using these [positional parameters](overlay.md#position-of-layer).

## Transformation of text overlay

Following transformation parameters are supported on the text inside a layer.

| Parameter                          | Description                  |
|------------------------------------|------------------------------|
| w                                  | Width of the whole text layer. It can also accept arithmetic expressions such as `bw_div_2`, or `bw_mul_0.8`. Learn more about arithmetic expressions [here](../arithmetic-expressions-in-transformations.md). |
| fs                                 | Font size. It can also accept arithmetic expressions such as `bh_div_20`, or `bh_mul_0.05`. Learn more about arithmetic expressions [here](../arithmetic-expressions-in-transformations.md).  |
| ff                                 | Font family |
| co                                 | Color  |
| ia                                 | Inner alignment. Accepts `left`, `right` and `center`. The default value is `center`. |
| pa                                 | Padding. It can also accept arithmetic expressions such as `bw_mul_0.05`, or `bw_mod_5`. Learn more about arithmetic expressions [here](../arithmetic-expressions-in-transformations.md). |
| al                                 | Alpha |
| tg                                 | Typography |
| [bg](../video-transformation/resize-crop-and-other-common-video-transformations.md#background-bg)                    | It is used to specify the background color in RGB Hex Code (e.g. FF0000) or an RGBA Code (e.g. FFAABB50) that must be used for the video. If you specify an 8 character background, the last two characters must be a number between 00 and 99 , which is used to indicate the opacity level of the background. `00` represents an opacity level of `0.00`, `01`  represents opacity level `0.01`, and so on. |
| [r](../video-transformation/resize-crop-and-other-common-video-transformations.md#radius-r)                                | It is used to control the radius of the corner. To get a circle or oval shape, set the value to `max`. |

# Add video over video

You can add video or GIF over a base video using video layer (`l-video`).

## Usage

```markup
https://ik.imagekit.io/demo/sample-video.mp4?tr=l-video,i-overlay.mp4,l-end
```

You can also control the position of video overlay using these [positional parameters](overlay.md#position-of-layer).

All supported [video formats](../video-transformation/README.md#supported-codecs-for-inputs) & GIF can be used a input (`i` or `ie`) for video layer.

{% hint style="info" %}
Animated-WebP and animated-PNG (`apng`) are not supported in Imagekit's video API.
{% endhint %}

## Transformation of video overlay

You can transform the layer video using any [supported video transformation parameter](../video-transformation/resize-crop-and-other-common-video-transformations.md) in ImageKit except `sr`.

# Add solid color blocks over video

You can add solid color blocks over a base video using the following example.

## Usage syntax

```markup
https://ik.imagekit.io/demo/sample-video.mp4?tr=l-image,i-ik_canvas,bg-red,l-end
```

You can also control the position of solid color blocks using these [positional parameters](overlay.md#position-of-layer).

## Transformation of solid color blocks overlay

Following transformation parameters are supported on the solid color block overlay inside a layer.

| Parameter                                                                                | Description                        |
|------------------------------------------------------------------------------------------|------------------------------------|
| w                                                                                        | Width of solid color block. It can also accept arithmetic expressions such as `bw_div_2`, or `bw_mul_0.8`. Learn more about arithmetic expressions [here](../arithmetic-expressions-in-transformations.md). |
| h                                                                                        | Height of solid color block. It can also accept arithmetic expressions such as `bh_div_2`, or `bh_mul_0.8`. Learn more about arithmetic expressions [here](../arithmetic-expressions-in-transformations.md).  |
| [bg](../image-transformations/resize-crop-and-other-transformations.md#background-bg) | It is used to specify the color of the block in RGB Hex Code (e.g. `FF0000`), or an RGBA Code (e.g. `FFAABB50`), or a color name (e.g. `red`). If you specify an 8 character background, the last two characters must be a number between `00` and `99`, which indicate the opacity level of the background. `00` represents an opacity level of `0.00`, `01` represents an opacity level of `0.01`, and so on. |
| al                                                                                       | It is used to specify the transparency level of the overlaid solid color layer. Supports integers from `1` to `9`. |
| [r](../image-transformations/resize-crop-and-other-transformations.md#radius-r)             | It is used to control the radius of the corner. To get a circle or oval shape, set the value to `max`. |

{% hint style="info" %}
If both `bg` and `al` are set in a single transformation and `bg` has an alpha component, then that value is used to set solid color background transparency. Otherwise, `al` value is used. If `bg` is set to a standard color name (e.g. `blue`), then the `al` value is ignored. Read more [here](../image-transformations/overlay.md#overlay-background-transparency)
{% endhint %}

# Add subtitles over a video

You can add subtitles over a base video using the following example.

## Usage syntax

```markup
https://ik.imagekit.io/demo/sample-video.mp4?tr=l-subtitles,i-english.srt,l-end
```
The position of subtitles cannot be controlled.

## Transformation of subtitles overlay

Transformation of subtitles are not supported.