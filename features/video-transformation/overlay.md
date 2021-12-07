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
* [Add subtitles over video](overlay.md#add-subtitles-over-video)

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
The input of the layer can be specified using `i` or `ie` (base54 encoded) parameter. In case both `i` and `ie` is present, `i` is ignored.

## Position of layer

The position of the layer can be controlled using the following parameters. The position of the layer is always relative to the immediate parent. For instance, the parent is the base video in the above example, and the logo image is the nested layer. 

{% hint style="info" %}
The position of subtitles cannot be controlled at this point.
{% endhint %}

| Parameter   | Description |
| ----------- | ----------- |
| lx          | `x` of the top-left corner in the base asset where layer's top-left corner would be placed.    |
| ly          | `y` of the top-left corner in the base asset where layer's top-left corner would be placed.    |
| lfo         | Position of layer in relative terms e.g. `center`, `top`, `left`, `bottom`, `right`, `top_left`, `top_right`, `bottom_left` and `bottom_right`.       |
| lso         | Start time of the base video in seconds when the layer should appear. |
| ldu         | Duration in seconds during which layer should appear on the base video. |
| leo         | End time of the base video when this layer should disappear. In case both `leo` and `ldu` are present, `ldu` is ignored.  |


## Transformation of layer

You can apply transformations to the layer as you would otherwise. However, any transformation parameter inside the layer is only applicable to that layer and not the parent.

However, different types of layers support different types of transformations which are covered in respective sections.

* [Transformation of images overlay](overlay.md#transformation-of-image-overlay)
* [Transformation of text overlay](overlay.md#transformation-of-text-overlay)
* [Transformation of video overlay](overlay.md#transformation-of-video-overlay)
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
You can add an image over a base video using the following example.

## Usage syntax

```markup
https://ik.imagekit.io/demo/base-video.mp4?tr=l-image,i-logo.png,l-end
```

You can also control the position of image overlay using these [positional parameters](overlay.md#position-of-layer).

## Transformation of image overlay

ImageKit supports many [image transformation parameters](../image-transformations/README.md), however inside a layer, you can apply the following transformations to the image overlay before it is placed on the base video. You can [chain the transformations](../image-transformations/chained-transformations.md) one after other to achieve the desired outcome.

| Parameter                                                                                                      | Description                  |
|----------------------------------------------------------------------------------------------------------------|------------------------------|
| [w](../image-transformations/resize-crop-and-other-transformations.md#width-w)                                 | Width of overlay image.  |
| [h](../image-transformations/resize-crop-and-other-transformations.md#height-h)                                | Height of overlay image. |
| [ar](../image-transformations/resize-crop-and-other-transformations.md#aspect-ratio-ar)                        | Apect ratio of overlay image. |
| [c](../image-transformations/resize-crop-and-other-transformations.md#crop-crop-modes-and-focus)               | Cropping method. Accepts `force`, `at_max`, and `at_least`. |
| [cm](../image-transformations/resize-crop-and-other-transformations.md#crop-crop-modes-and-focus)              | Crom mode. Supports `extract` and `pad_resize`. |
| `x`, `y`, `xc`, and `yc` to control [custom coordinates](../image-transformations/resize-crop-and-other-transformations.md#example-focus-using-custom-coordinates).  | Control the custom coordinates for focus point. |
| [fo](../image-transformations/resize-crop-and-other-transformations.md#focus-fo)                               | Relative focus area used during cropping. Accpets `center`, `top`, `left`, `bottom`, `right`, `top_left`, `top_right`, `bottom_left` and `bottom_right`. |
| [bl](../image-transformations/resize-crop-and-other-transformations.md#blur-bl)                                | Blur the overlay image  |
| [dpr](../image-transformations/resize-crop-and-other-transformations.md#dpr-dpr)                               | Specify the device pixel ratio that is used to calculate the dimensions of the output image. The `dpr` parameter can only be used when either the height or width of the desired output image is specified.                             |
| [b](../image-transformations/resize-crop-and-other-transformations.md#border-b)                                | This adds a border to the overlay image. It accepts two parameters - the width of the border and the color of the border in format `b-<border-width>-<hex code>` |
| [bg](../image-transformations/resize-crop-and-other-transformations.md#background-color-bg)                    | It is used to specify the background color in RGB Hex Code (e.g. FF0000) or an RGBA Code (e.g. FFAABB50) that must be used for the image. If you specify an 8 character background, the last two characters must be a number between 00 and 99 , which is used to indicate the opacity level of the background. `00` represents an opacity level of `0.00`, `01`  represents opacity level `0.01`, and so on. |
| [r](../image-transformations/resize-crop-and-other-transformations.md#radius-r)                                | It is used to control the radius of the corner. To get a circle or oval shape, set the value to `max`. |


# Add text over video

You can add any text string over a base video using the following example.

## Usage syntax

```markup
https://ik.imagekit.io/demo/base-video.mp4?tr=l-text,i-hello,l-end
```

You can also control the position of text overlay using these [positional parameters](overlay.md#position-of-layer).

## Transformation of text overlay

Following transformation parameters are supported on the text inside a layer.

| Parameter                          | Description                  |
|------------------------------------|------------------------------|
| w                                  | Width of the whole text layer.  |
| h                                  | Height of the whole text layer.  |
| fs                                 | Font size  |
| ff                                 | Font family |
| co                                 | Color  |
| ia                                 | Inner alignment. Accepts `left`, `right` and `center`. The default value is `center`. |
| pa                                 | Padding |
| al                                 | Alpha |
| tg                                 | Typography |
| [bg](../video-transformation/resize-crop-and-other-common-video-transformations.md#background-color-bg)                    | It is used to specify the background color in RGB Hex Code (e.g. FF0000) or an RGBA Code (e.g. FFAABB50) that must be used for the video. If you specify an 8 character background, the last two characters must be a number between 00 and 99 , which is used to indicate the opacity level of the background. `00` represents an opacity level of `0.00`, `01`  represents opacity level `0.01`, and so on. |
| [r](../video-transformation/resize-crop-and-other-common-video-transformations.md#radius-r)                                | It is used to control the radius of the corner. To get a circle or oval shape, set the value to `max`. |

# Add video over video

You can add video over a base video using the following example.

## Usage syntax

```markup
https://ik.imagekit.io/demo/base-video.mp4?tr=l-video,i-overlay.mp4,l-end
```

You can also control the position of video overlay using these [positional parameters](overlay.md#position-of-layer).

## Transformation of video overlay

Following transformation parameters are supported on the video inside a layer.

| Parameter                                                                                                      | Description                  |
|----------------------------------------------------------------------------------------------------------------|------------------------------|
| [w](../video-transformation/resize-crop-and-other-common-video-transformations.md#width-w)                                 | Width of overlay video.  |
| [h](../video-transformation/resize-crop-and-other-common-video-transformations.md#height-h)                                | Height of overlay video. |
| [ar](../video-transformation/resize-crop-and-other-common-video-transformations.md#aspect-ratio-ar)                        | Apect ratio of overlay video. |
| [c](../video-transformation/resize-crop-and-other-common-video-transformations.md#crop-crop-modes-and-focus)               | Cropping method. Accepts `force`, `at_max`, and `at_least`. |
| [cm](../video-transformation/resize-crop-and-other-common-video-transformations.md#crop-crop-modes-and-focus)              | Crom mode. Supports `extract` and `pad_resize`. |
| `x`, `y`, `xc`, and `yc` to control [custom coordinates](../video-transformation/resize-crop-and-other-common-video-transformations.md#example-focus-using-custom-coordinates).  | Control the custom coordinates for focus point. |
| [fo](../video-transformation/resize-crop-and-other-common-video-transformations.md#focus-fo)                               | Relative focus area used during cropping. Accpets `center`, `top`, `left`, `bottom`, `right`, `top_left`, `top_right`, `bottom_left` and `bottom_right`. |
| [so](../video-transformation/resize-crop-and-other-common-video-transformations.md#start-offset-so)         | Start offset in seconds in overlay video. Video before `so` time will be trimmed. |
| [du](../video-transformation/resize-crop-and-other-common-video-transformations.md#duration-du)        | Duration in seconds of overlay video. After this, remaining video will be trimmed. |
| [eo](../video-transformation/resize-crop-and-other-common-video-transformations.md#end-offset-eo)       | End offset in seconds in overlay video. Video after `eo` time will be trimmed. In case both `eo` and `du` is present, `du` is ignored.  |
| [bl](../video-transformation/resize-crop-and-other-common-video-transformations.md#blur-bl)                                | Blur the overlay video  |
| [dpr](../video-transformation/resize-crop-and-other-common-video-transformations.md#dpr-dpr)                               | Specify the device pixel ratio that is used to calculate the dimensions of the output video. The `dpr` parameter can only be used when either the height or width of the desired output video is specified.                             |
| [b](../video-transformation/resize-crop-and-other-common-video-transformations.md#border-b)                                | This adds a border to the overlay video. It accepts two parameters - the width of the border and the color of the border in format `b-<border-width>-<hex code>` |
| [bg](../video-transformation/resize-crop-and-other-common-video-transformations.md#background-color-bg)                    | It is used to specify the background color in RGB Hex Code (e.g. FF0000) or an RGBA Code (e.g. FFAABB50) that must be used for the video. If you specify an 8 character background, the last two characters must be a number between 00 and 99 , which is used to indicate the opacity level of the background. `00` represents an opacity level of `0.00`, `01`  represents opacity level `0.01`, and so on. |
| [r](../video-transformation/resize-crop-and-other-common-video-transformations.md#radius-r)                                | It is used to control the radius of the corner. To get a circle or oval shape, set the value to `max`. |

# Add subtitles over video

You can add subtitles over a base video using the following example.

## Usage syntax

```markup
https://ik.imagekit.io/demo/base-video.mp4?tr=l-subtitles,i-english.srt,l-end
```
The position of subtitles cannot be controlled in the beta version.

## Transformation of subtitles overlay

Transformation of subtitles are not supported in the beta version.