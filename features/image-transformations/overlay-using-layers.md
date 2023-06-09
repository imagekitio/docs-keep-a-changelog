---
description: >-
  A comprehensive guide that covers how you can add images and text over
  a base image.
---

# Overlay using layers (beta)

{% hint style="info" %}
Overlay on images using layers is currently in beta.
{% endhint %}

In ImageKit, you can add images and text over a base image using [layers](#layers). Skip to the relevant section to understand with a quick example:

* [Add images over image](overlay-using-layers#add-images-over-image)
* [Add text over image](overlay-using-layers#add-text-over-image)
* [Add solid color blocks over image](overlay-using-layers#add-images-over-image)

# Layers
A layer is a special kind of transformation that allows you to modify the overlay itself and express its position in relation to the parent.

## Syntax of layers

A layer starts with `l-<type>` and ends with `l-end`. All the positional and transformation parameters of that layer are between `l-<type>` and `l-end` and only apply to that layer and not the parent base asset.

`type` can be `image` or `text`.

For example, in the following URL, we are adding a logo image `logo.png` on top of a base image, i.e. `sample-image.jpg`. However, we applied width (`w-10`) and rotation (`rt-90`) transformations on this overlay logo image before placing it on the base image. Transformations `w-300,h-300` are applied to `sample-image.jpg`.

Here the parent base image has one layer inside it. A layer can also nest another layer.

```markup
        URL-endpoint           Base image                                   Layer
┌──────────────────────────┐┌──────────────┐                ┌─────────────────────────────────┐
https://ik.imagekit.io/demo/sample-image.jpg?tr=w-300,h-300,l-image,i-logo.png,w-10,rt-90,l-end
                                                └────┬────┘                    └────┬────┘
                                                     │                              │
                                                     │        These transformations are applied to logo.png
                                                     │
                           These transformations are applied to sample-image.jpg
```

## Input of layer
The input of the layer can be specified using `i` or `ie` (base64 encoded) parameter. In case both `i` and `ie` is present, `i` is ignored. The base64 string should be made URL safe to ensure that all padding characters (=) are included correctly. In Javascript, a function like `encodeURIComponent()` can be used for this.

## Position of layer

The position of the layer can be controlled using the following parameters. The position of the layer is always relative to the immediate parent. For instance, the parent is the base image in the above example, and the logo image is the nested layer. 

| Parameter   | Description |
| ----------- | ----------- |
| lx          | `x` of the top-left corner in the base asset where layer's top-left corner would be placed.    |
| ly          | `y` of the top-left corner in the base asset where layer's top-left corner would be placed.    |
| lfo         | Position of layer in relative terms e.g. `center`, `top`, `left`, `bottom`, `right`, `top_left`, `top_right`, `bottom_left` and `bottom_right`. Default value is `center`.     |

## Transformation of layer

You can apply transformations to the layer as you would otherwise. However, any transformation parameter inside the layer is only applicable to that layer and not the parent.

However, different types of layers support different types of transformations which are covered in respective sections.

* [Transformation of images overlay](overlay-using-layers#transformation-of-image-overlay)
* [Transformation of text overlay](overlay-using-layers#transformation-of-text-overlay)

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

# Add images over image
You can add an image over a base image using the following example.

## Usage syntax

```markup
https://ik.imagekit.io/demo/base-image.jpg?tr=l-image,i-logo.png,l-end
```

You can also control the position of image overlay using these [positional parameters](overlay-using-layers#position-of-layer).

## Transformation of image overlay

ImageKit supports many [image transformation parameters](../image-transformations/README.md), however inside a layer, you can apply the following transformations to the image overlay before it is placed on the base image. You can [chain the transformations](../image-transformations/chained-transformations.md) one after other to achieve the desired outcome.

| Parameter                                                                                                   | Description                  |
|-------------------------------------------------------------------------------------------------------------|------------------------------|
| [w](../image-transformations/resize-crop-and-other-transformations#width-w)                                 | Width of overlay image.  |
| [h](../image-transformations/resize-crop-and-other-transformations#height-h)                                | Height of overlay image. |
| [ar](../image-transformations/resize-crop-and-other-transformations#aspect-ratio-ar)                        | Apect ratio of overlay image. |
| [c](../image-transformations/resize-crop-and-other-transformations#crop-crop-modes-and-focus)               | Cropping method. Accepts `force`, `at_max`, and `at_least`. |
| [cm](../image-transformations/resize-crop-and-other-transformations#crop-crop-modes-and-focus)              | Crop mode. Supports `extract` and `pad_resize`. |
| [fo](../image-transformations/resize-crop-and-other-transformations#focus-fo)                               | Relative focus area used during cropping. Accpets `center`, `top`, `left`, `bottom`, `right`, `top_left`, `top_right`, `bottom_left` and `bottom_right`. |
| [b](../image-transformations/resize-crop-and-other-transformations#border-b)                                | This adds a border to the overlay image. It accepts two parameters - the width of the border and the color of the border in format `b-<border-width>-<hex code>` |
| [bg](../image-transformations/resize-crop-and-other-transformations#background-color-bg)                    | It is used to specify the background color in RGB Hex Code (e.g. FF0000) or an RGBA Code (e.g. FFAABB50) that must be used for the image. If you specify an 8 character background, the last two characters must be a number between 00 and 99 , which is used to indicate the opacity level of the background. `00` represents an opacity level of `0.00`, `01`  represents opacity level `0.01`, and so on. |
| [r](../image-transformations/resize-crop-and-other-transformations#radius-r)                                | It is used to control the radius of the corner. To get a circle or oval shape, set the value to `max`. |
| [rt](../image-transformations/resize-crop-and-other-transformations#rotate-rt)                              | It is used to specify the degree by which the overlay image must be rotated. To get a circle or oval shape, set the value to `max`. |
| [t](../image-transformations/resize-crop-and-other-transformations#trim-edges-t)                            | By default, ImageKit.io trims the overlay image before overlaying it on the base image. Trimming removes the similar colored pixels from the edges. To prevent the default trimming from happening, use `t-false`. This removes the transparency of the overlaid image. **Possible values** include `true`, `false` and integer values between `1` and `99` that specify the threshold level for considering a particular pixel as "background". |
| [dpr](../image-transformations/resize-crop-and-other-transformations#dpr-dpr)                               | It is used to specify the device pixel ratio that is used to calculate the dimensions of the overlay image. It can only be used when either the height or width of the desired output image is specified. The possible values are `0.1` to `5`, or `auto`. |
| [q](../image-transformations/resize-crop-and-other-transformations#quality-q)                               | It is used to specify the quality of the output image for lossy formats like JPEG, WebP and AVIF. A large quality number indicates a larger output image size with high quality. A small quality number indicates a smaller output image size with lower quality. It can be a whole number between `1` and `100`. Default value is `80`. |

# Add text over image

You can add any text string over a base image using the following examples. 

You can also control the position of text overlay using these [positional parameters](overlay-using-layers#position-of-layer).

## Usage syntax

### Plain text overlay

Specified with the `i-` parameter in a `text` layer. Supported characters include:
- All alphabets and numbers (including Unicode letters, marks, and numerals in other languages) 
- Spaces 
- Special characters including \_, -, %, !, @, and &

```markup
https://ik.imagekit.io/demo/base-image.jpg?tr=l-text,i-hello,l-end
```

### Base64 encoded text overlay

Specified with the `ie-` parameter in a `text` layer. It accepts a base64 string as input, allowing you to overlay special unicode characters (e.g. ©, ®, ™ etc.) that you cannot otherwise pass in a URL as plain string.

{% hint style="info" %}
1. The base64 string should be made URL safe to ensure that all padding characters(=) are included correctly. In Javascript, a function like `encodeURIComponent()` can be used for this.
2. Input above the length of 144 characters will be truncated.
{% endhint %}

```markup
https://ik.imagekit.io/demo/base-image.jpg?tr=l-text,ie-aGVsbG8%3D,l-end
```
Here `aGVsbG8%3D` is the base64 string for "hello".

## Transformation of text overlay

Following transformation parameters are supported on the text inside a layer.

| Parameter                          | Non-layer syntax equivalent                           | Description                           | 
|------------------------------------|-------------------------------------------------------|---------------------------------------|
| w                                  | [otw](./overlay#overlay-text-width---otw)             | The maximum width (in pixels) of the overlaid text on the image. The text is wrapped so that any words in a line that go beyond the given width are sent to the next line. Height of the text box is calculated automatically based on the total number of lines. |
| fs                                 | [ots](./overlay#overlay-text-size---ots)              | It is used to specify the font size.  |
| ff                                 | [otf](./overlay#overlay-text-font---otf)              | It is used to specify the font of the overlaid text on the image. You can choose [any font from this list](supported-text-font-list#in-built-fonts) or use a [custom font](overlay#using-custom-fonts).                        |
| co                                 | [otc](./overlay#overlay-text-color---otc)             | It is used to specify the color and transparency of the overlaid text on the image. It accepts RGB Hex Codes (e.g. `FF0000`) or RGBA Codes (e.g. `FFAABB50`) or color names (e.g. `red`). If you specify an 8 character background, the last two characters must be a number between `00` and `99`, which indicate the opacity level of the background. `00` represents an opacity level of `0.00`, `01` represents opacity level `0.01`, and so on. |
| ia                                 | [otia](./overlay#overlay-text-inner-alignment---otia) | Inner alignment. Accepts `left`, `right` and `center`. The default value is `center`. |
| pa                                 | [otp](./overlay#overlay-text-padding---otp)           | It is used to specify the padding around the overlaid text on the image. Supports any positive integer or a set of positive integers separated by underscores. The set of integers follow CSS shorthand order for determining the padding along each side of the overlay. e.g. `10`, `10_20`, `10_20_30` or `10_20_30_40`.         |
| al                                 | [oa](./overlay#overlay-transparency---oa)             | It is used to specify the transparency level of the overlaid text layer. Supports integers from `1` to `9`.                          |
| tg                                 | [ott](./overlay#overlay-text-typography---ott)        | Typography. Supports bold `b`, italics `i`, and bold with italics `b_i` |
| [bg](../image-transformations/resize-crop-and-other-transformations#background-color-bg) | [otbg](./overlay#overlay-text-background---otbg) | It is used to specify the background color in RGB Hex Code (e.g. `FF0000`) or an RGBA Code (e.g. `FFAABB50`) or a color name (e.g. `red`). If you specify an 8 character background, the last two characters must be a number between `00` and `99`, which indicate the opacity level of the background. `00` represents an opacity level of `0.00`, `01` represents opacity level `0.01`, and so on. |
| [r](../image-transformations/resize-crop-and-other-transformations#radius-r)             | [or](./overlay#overlay-radius---or)              | It is used to control the radius of the corner. To get a circle or oval shape, set the value to `max`. |

## Differences between overlay text output in layer and non-layer syntax

In overlay text, the output generated using the layers syntax may be slightly different from its non-layer syntax equivalent. If you are moving from non-layer overlay syntax to layer syntax, you may need to adjust some transformation parameter values to get a similar output as before.

These differences include but are not limited to:

- **Top and bottom padding:** Apparent empty space (or padding) on the top and bottom of overlay text will be slightly lesser in layer syntax than non-layer syntax. It can be handled by increasing the top and bottom padding accordingly using the `pa` layer transformation parameter.
- **Trimming:** Any empty space around the text output will be trimmed by default in the layer syntax output. It can be handled by adding the padding manually using the `pa` layer transformation parameter.
- **Line height:** The line height of the text will be marginally larger in layer syntax. It cannot be controlled manually for the moment.
- **Line breakpoints:** The points at which sentences break when maximum width is used may be different. It cannot be controlled manually.
- **Unicode language support:** Layer overlay text supports Unicode letters, marks, and numerals in other languages, while non-layer overlay text does not. 

# Add solid color blocks over image
You can add a solid color block over a base image using the following example.

## Usage syntax

```markup
https://ik.imagekit.io/demo/base-image.jpg?tr=l-image,i-ik_canvas,bg-FF0000,w-300,h-100,l-end
```

You can also control the position of image overlay using these [positional parameters](overlay-using-layers#position-of-layer).

## Transformation of solid color overlay

Following transformation parameters are supported on the solid color background inside a layer.

| Parameter                                                                                | Description                        |
|------------------------------------------------------------------------------------------|------------------------------------|
| w                                                                                        | Width of solid color background.   |
| h                                                                                        | Height of solid color background.  |
| [bg](../image-transformations/resize-crop-and-other-transformations#background-color-bg) | It is used to specify the solid color background color in RGB Hex Code (e.g. `FF0000`) or an RGBA Code (e.g. `FFAABB50`) or a color name (e.g. `red`). If you specify an 8 character background, the last two characters must be a number between `00` and `99`, which indicate the opacity level of the background. `00` represents an opacity level of `0.00`, `01` represents opacity level `0.01`, and so on. |
| al                                                                                       | It is used to specify the transparency level of the overlaid solid color layer. Supports integers from `1` to `9`. |
| [r](../image-transformations/resize-crop-and-other-transformations#radius-r)             | It is used to control the radius of the corner. To get a circle or oval shape, set the value to `max`. |

{% hint style="info" %}
If both `bg` and `al` are set in a single transformation and `bg` has an alpha component, then that value is used to set solid color background transparency. Otherwise, `al` value is used. If `bg` is set to a standard color name (eg. `blue`), then the `al` value is ignored. Read more [here](../image-transformations/overlay#overlay-background-transparency)
{% endhint %}
