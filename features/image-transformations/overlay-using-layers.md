---
description: >-
  A comprehensive guide that covers how you can add images and text over
  a base image using the new layer syntax that supports overlay nesting
  and provides better positional control.
---

# Overlay using layers

With ImageKit, you can add images and text over a base image using [layers](#layers). Skip to the relevant section to understand with a quick example:

* [Add images over image](overlay-using-layers.md#add-images-over-image)
* [Add text over image](overlay-using-layers.md#add-text-over-image)
* [Add solid color blocks over image](overlay-using-layers.md#add-solid-color-blocks-over-image)

You can also learn from more [advanced examples](#examples). Please check [expensive transformation limits](/limits-and-troubleshooting/limits.md#expensive-transformation-limits) before using this in production.

# Layers
A layer is a special kind of transformation in which you can specify an asset to be used as an overlay, along with its positioning and transformations. It supports nesting, allows you to modify the overlay itself, and express its position in relation to the parent.

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
The input of the layer can be specified using `i` or `ie` (base64 encoded) parameter. In case both `i` and `ie` are present, `i` is ignored. The base64 string should be made URL safe to ensure that all padding characters (=) are included correctly. In Javascript, a function like `encodeURIComponent()` can be used for this.

## Position of layer

The position of the layer can be controlled using the following parameters. The position of the layer is always relative to the immediate parent. For instance, the parent is the base image in the above example, and the logo image is the nested layer. 

| Parameter   | Description |
| ----------- | ----------- |
| lx          | `x` of the top-left corner in the base asset where the layer's top-left corner would be placed.    |
| ly          | `y` of the top-left corner in the base asset where the layer's top-left corner would be placed.    |
| lfo         | Position of the layer in relative terms e.g., `center`, `top`, `left`, `bottom`, `right`, `top_left`, `top_right`, `bottom_left`, and `bottom_right`. The default value is `center`.     |

**Note:** If one or both of `lx` and `ly` parameters are specified along with `lfo`, then `lfo` parameter is ignored.

## Transformation of layer

You can apply transformations to the layer as you would on the base asset. However, any transformation parameter inside the layer is only applicable to that layer and not the parent.

However, different types of layers support different types of transformations, which are covered in respective sections.

* [Transformation of images overlay](overlay-using-layers.md#transformation-of-image-overlay)
* [Transformation of text overlay](overlay-using-layers.md#transformation-of-text-overlay)
* [Transformation of solid color overlay](overlay-using-layers.md#transformation-of-solid-color-overlay)

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

You can also control the position of the image overlay using these [positional parameters](overlay-using-layers.md#position-of-layer).

## Transformation of image overlay

ImageKit supports many [image transformation parameters](../image-transformations/README.md). However, inside a layer, you can apply the following transformations to the image overlay before it is placed on the base image. You can [chain the transformations](../image-transformations/chained-transformations.md) one after the other to achieve the desired outcome.

| Parameter                                                                                                   | Description                  |
|-------------------------------------------------------------------------------------------------------------|------------------------------|
| [w](../image-transformations/resize-crop-and-other-transformations.md#width-w)                                 | Width of the overlay image.  |
| [h](../image-transformations/resize-crop-and-other-transformations.md#height-h)                                | Height of the overlay image. |
| [ar](../image-transformations/resize-crop-and-other-transformations.md#aspect-ratio-ar)                        | Aspect ratio of the overlay image. |
| [c](../image-transformations/resize-crop-and-other-transformations.md#crop-crop-modes-and-focus)               | Cropping method. Accepts `force`, `at_max`, and `at_least`. |
| [cm](../image-transformations/resize-crop-and-other-transformations.md#crop-crop-modes-and-focus)              | Crop mode. Supports `extract` and `pad_resize`. |
| [fo](../image-transformations/resize-crop-and-other-transformations.md#focus-fo)                               | Relative focus area used during cropping. Accepts `face`, `custom`, `center`, `top`, `left`, `bottom`, `right`, `top_left`, `top_right`, `bottom_left`, and `bottom_right`. |
| [z](../image-transformations/resize-crop-and-other-transformations.md#zoom---z)                                | It determines how much to zoom in or out of the cropped area. It must be used along with `fo-face`. A value less than `1.0` zooms out to include more background surrounding the face, whereas a value larger than `1.0` zooms in to exclude more background surrounding the face. Possible values include any number between `0` to `1` for zoom out, and greater than `1` for zoom in. Its default value is `1.0`. |
| [b](../image-transformations/resize-crop-and-other-transformations.md#border-b)                                | This adds a border to the overlay image. It accepts two parameters - the width of the border and the color of the border in format `b-<border-width>-<hex code>` |
| [bg](../image-transformations/resize-crop-and-other-transformations.md#background-color-bg)                    | It is used to specify the background color in RGB Hex Code (e.g. `FF0000`), or an RGBA Code (e.g. `FFAABB50`), or a color name (e.g. `red`) that must be used for the image. If you specify an 8 character background, the last two characters must be a number between 00 and 99, which is used to indicate the opacity level of the background. `00` represents an opacity level of `0.00`, `01` represents an opacity level of `0.01`, and so on. |
| [r](../image-transformations/resize-crop-and-other-transformations.md#radius-r)                                | It is used to control the radius of the corner. To get a circle or oval shape, set the value to `max`. |
| [rt](../image-transformations/resize-crop-and-other-transformations.md#rotate-rt)                              | It is used to specify the degree by which the overlay image must be rotated. |
| [t](../image-transformations/resize-crop-and-other-transformations.md#trim-edges-t)                            | By default, ImageKit.io trims the overlay image before overlaying it on the base image. Trimming removes similar colored pixels from the edges. To prevent the default trimming from happening, use `t-false`. **Possible values** include `true`, `false`, and integer values between `1` and `99` that specifies the threshold level for considering a particular pixel as "background". |
| [dpr](../image-transformations/resize-crop-and-other-transformations.md#dpr-dpr)                               | It is used to specify the device pixel ratio that is used to calculate the dimensions of the overlay image. It can only be used when either the height or width of the desired output image is specified. The possible values are `0.1` to `5`, or `auto`. |
| [q](../image-transformations/resize-crop-and-other-transformations.md#quality-q)                               | It is used to specify the quality of the output image for lossy formats like JPEG, WebP, and AVIF. A large quality number indicates a larger output image size with high quality. A small quality number indicates a smaller output image size with lower quality. It can be a whole number between `1` and `100`. The default value is `80`. |
| [bl](../image-transformations/resize-crop-and-other-transformations.md#blur---bl)                               | It is used to specify the gaussian blur that must be applied to an image. The value of `bl` specifies the radius of the Gaussian Blur that is to be applied. The higher the value, the larger the radius of Gaussian Blur. Accepts integers between `1` and `100`. |
| [e-grayscale](../image-transformations/resize-crop-and-other-transformations.md#grayscale---e-grayscale)       | It is used to turn an image to a grayscale version. |
| [e-contrast](../image-transformations/image-enhancement-and-color-manipulation.md#contrast-stretch---e-contrast) | It is used to automatically enhance the contrast of the image by using the full intensity values that particular image format allows. This means that the lighter sections of an image become even lighter and the darker sections becomes even brighter, thereby enhancing the contrast. |
| [e-sharpen](../image-transformations/image-enhancement-and-color-manipulation.md#sharpen---e-sharpen)          | It is used to sharpen the input image, useful when highlighting the edges and finer details within an image. If just the `e-sharpen` parameter is used, then a default sharpening is performed on the input image. This can be controlled by specifying a number that restricts the extent of sharpening performed, like `e-sharpen-<number>`. |
| [e-usm](../image-transformations/image-enhancement-and-color-manipulation.md#unsharp-mask---e-usm)             | It is used to apply and control unsharp masks on your images. The amount of sharpening can be varied using 4 parameters - `radius`, `sigma`, `amount`, and `threshold`. This results in perceptually better images compared to using `e-sharpen`. Only positive floating points are allowed for these 4 parameters. e.g. `e-usm-<radius>-<sigma>-<amount>-<threshold>` |


# Add text over image

You can add any text string over a base image using the following examples. 

You can also control the position of the text overlay using these [positional parameters](overlay-using-layers.md#position-of-layer).

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

It is specified with the `ie-` parameter in a `text` layer. It accepts a base64 string as input, allowing you to overlay special Unicode characters (e.g. ©, ®, ™, etc.) that you cannot otherwise pass in a URL as a plain string.

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
| w                                  | [otw](./overlay.md#overlay-text-width---otw)             | The maximum width (in pixels) of the overlaid text on the image. The text is wrapped so that any words in a line that go beyond the given width are sent to the next line. The height of the text box is calculated automatically based on the total number of lines. |
| fs                                 | [ots](./overlay.md#overlay-text-size---ots)              | It is used to specify the font size.  |
| ff                                 | [otf](./overlay.md#overlay-text-font---otf)              | It is used to specify the font of the overlaid text on the image. You can choose [any font from this list](supported-text-font-list.md#in-built-fonts) or use a [custom font](overlay-using-layers.md#using-custom-fonts-in-text-overlays).                        |
| co                                 | [otc](./overlay.md#overlay-text-color---otc)             | It is used to specify the color and transparency of the overlaid text on the image. It accepts RGB Hex Codes (e.g. `FF0000`), or RGBA Codes (e.g. `FFAABB50`), or color names (e.g. `red`). If you specify an 8 character background, the last two characters must be a number between `00` and `99`, which indicate the opacity level of the background. `00` represents an opacity level of `0.00`, `01` represents an opacity level of `0.01`, and so on. |
| ia                                 | [otia](./overlay.md#overlay-text-inner-alignment---otia) | Inner alignment. Accepts `left`, `right`, and `center`. The default value is `center`. |
| pa                                 | [otp](./overlay.md#overlay-text-padding---otp)           | It is used to specify the padding around the overlaid text on the image. Supports any positive integer or a set of positive integers separated by underscores. The set of integers follow CSS shorthand order for determining the padding along each side of the overlay. e.g. `10`, `10_20`, `10_20_30` or `10_20_30_40`.         |
| al                                 | [oa](./overlay.md#overlay-transparency---oa)             | It is used to specify the transparency level of the overlaid text layer. Supports integers from `1` to `9`.                          |
| tg                                 | [ott](./overlay.md#overlay-text-typography---ott)        | Typography. Supports bold `b`, italics `i`, and bold with italics `b_i` |
| [bg](../image-transformations/resize-crop-and-other-transformations.md#background-color-bg) | [otbg](./overlay.md#overlay-text-background---otbg) | It is used to specify the background color in RGB Hex Code (e.g. `FF0000`) or an RGBA Code (e.g. `FFAABB50`), or a color name (e.g. `red`). If you specify an 8 character background, the last two characters must be a number between `00` and `99`, which indicate the opacity level of the background. `00` represents an opacity level of `0.00`, `01` represents an opacity level of `0.01`, and so on. |
| [r](../image-transformations/resize-crop-and-other-transformations.md#radius-r)             | [or](./overlay.md#overlay-radius---or)              | It is used to control the radius of the corner. To get a circle or oval shape, set the value to `max`. |

### Using custom fonts in text overlays

To use a custom font in text overlays, upload the font file in your media library and pass its path in the `ff` parameter in a text layer.

```markup
https://ik.imagekit.io/demo/tr:l-text,i-Hello%20World,ff-Lacquer-Regular_nGqtVsBJT.ttf,fs-72,co-FFF000,l-end/default-image.jpg
```

Here the font file is available at path `/Lacquer-Regular_nGqtVsBJT.ttf` in the media library. You will need to pass the whole path of the custom font, including its extension.

If your custom font file is in a nested folder, then replace the slashes `/`in the path with `@@`. For example, a font file available at path `/font-folder/font-subfolder/my-custom-font.ttf` will be used as follows:

```markup
https://ik.imagekit.io/demo/tr:l-text,i-Hello%20World,ff-font-folder@@font-subfolder@@my-custom-font.ttf,fs-72,co-FFF000,l-end/default-image.jpg
```

## Differences between overlay text output in layer and non-layer syntax

In overlay text, the output generated using the layers syntax may be slightly different from its non-layer syntax equivalent. If you are moving from non-layer overlay syntax to layer syntax, you may need to adjust some transformation parameter values to get a similar output as before.

These differences include but are not limited to:

- **Top and bottom padding:** Apparent empty space (or padding) on the top and bottom of overlay text will be slightly lesser in layer syntax than non-layer syntax. It can be handled by increasing the top and bottom padding accordingly using the `pa` layer transformation parameter.
- **Trimming:** Any empty space around the text output will be trimmed by default in the layer syntax output. It can be handled by adding the required padding using the `pa` layer transformation parameter.
- **Line height:** The line height of the text will be marginally larger in layer syntax. It cannot be controlled manually for the moment.
- **Line breakpoints:** The points at which sentences break when maximum width is used may be different. It cannot be controlled manually.
- **Unicode language support:** Layer overlay text supports Unicode letters, marks, and numerals in other languages, while non-layer overlay text does not. 

# Add solid color blocks over image
You can add a solid color block over a base image using the following example.

## Usage syntax

```markup
https://ik.imagekit.io/demo/base-image.jpg?tr=l-image,i-ik_canvas,bg-FF0000,w-300,h-100,l-end
```

You can also control the position of the solid color overlay using these [positional parameters](overlay-using-layers.md#position-of-layer).

## Transformation of solid color overlay

Following transformation parameters are supported on the solid color block overlay inside a layer.

| Parameter                                                                                | Description                        |
|------------------------------------------------------------------------------------------|------------------------------------|
| w                                                                                        | Width of solid color block.   |
| h                                                                                        | Height of solid color block.  |
| [bg](../image-transformations/resize-crop-and-other-transformations.md#background-color-bg) | It is used to specify the color of the block in RGB Hex Code (e.g. `FF0000`), or an RGBA Code (e.g. `FFAABB50`), or a color name (e.g. `red`). If you specify an 8 character background, the last two characters must be a number between `00` and `99`, which indicate the opacity level of the background. `00` represents an opacity level of `0.00`, `01` represents an opacity level of `0.01`, and so on. |
| al                                                                                       | It is used to specify the transparency level of the overlaid solid color layer. Supports integers from `1` to `9`. |
| [r](../image-transformations/resize-crop-and-other-transformations.md#radius-r)             | It is used to control the radius of the corner. To get a circle or oval shape, set the value to `max`. |

{% hint style="info" %}
If both `bg` and `al` are set in a single transformation and `bg` has an alpha component, then that value is used to set solid color background transparency. Otherwise, `al` value is used. If `bg` is set to a standard color name (e.g. `blue`), then the `al` value is ignored. Read more [here](../image-transformations/overlay.md#overlay-background-transparency)
{% endhint %}

# Examples

## Image overlay

### Basic position control

You can overlay an image using the `l-image` parameter and control its position using the `lx` and `ly` parameters. If the `lx` or `ly` value exceeds the dimensions of the input image, the values are adjusted to fit within the image. To change the direction, negative values are also supported by prefixing the value with 'N'.

{% tabs %}
{% tab title="Default center overlay" %}
URL - [https://ik.imagekit.io/demo/tr:l-image,i-logo-white_SJwqB4Nfe.png,l-end/medium_cafe_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:l-image,i-logo-white_SJwqB4Nfe.png,l-end/medium_cafe_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:l-image,i-logo-white_SJwqB4Nfe.png,l-end/medium_cafe_B1iTdD0C.jpg)
{% endtab %}

{% tab title="LX=35" %}
URL - [https://ik.imagekit.io/demo/tr:l-image,i-logo-white_SJwqB4Nfe.png,lx-35,l-end/medium_cafe_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:l-image,i-logo-white_SJwqB4Nfe.png,lx-35,l-end/medium_cafe_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:l-image,i-logo-white_SJwqB4Nfe.png,lx-35,l-end/medium_cafe_B1iTdD0C.jpg)
{% endtab %}

{% tab title="LX=N35 (Negative value)" %}
URL - [https://ik.imagekit.io/demo/tr:l-image,i-logo-white_SJwqB4Nfe.png,lx-N35,l-end/medium_cafe_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:l-image,i-logo-white_SJwqB4Nfe.png,lx-N35,l-end/medium_cafe_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:l-image,i-logo-white_SJwqB4Nfe.png,lx-N35,l-end/medium_cafe_B1iTdD0C.jpg)
{% endtab %}

{% endtabs %}

### Nesting of image layers

You can overlay multiple images by nesting them using the `l-image` parameter. In this example, we overlay the resized `women-dress.jpeg` with a red border on the bottom left corner of the base image and then add the `imagekit.io` logo on the top right corner of the `women-dress.jpeg` image. `lfo` parameter is used to control the position of layer in relative terms.

URL - [https://ik.imagekit.io/demo/tr:l-image,i-women-dress.jpeg,h-400,b-10_red,lfo-bottom_left,l-image,i-logo-white_SJwqB4Nfe.png,w-150,lfo-top_right,l-end,l-end/medium_cafe_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:l-image,i-women-dress.jpeg,h-400,b-10_red,lfo-bottom_left,l-image,i-logo-white_SJwqB4Nfe.png,w-150,lfo-top_right,l-end,l-end/medium_cafe_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:l-image,i-women-dress.jpeg,h-400,b-10_red,lfo-bottom_left,l-image,i-logo-white_SJwqB4Nfe.png,w-150,lfo-top_right,l-end,l-end/medium_cafe_B1iTdD0C.jpg)

### Transformation on image layer
You can apply various transformations to the image layer using the supported transformation parameters. For example, you can use the `cm-pad_resize` cropping with a yellow background to fill the image layer's background.

URL - [https://ik.imagekit.io/demo/tr:l-image,i-women-dress.jpeg,h-400,w-400,cm-pad_resize,bg-yellow,lfo-bottom_left,l-image,i-logo-white_SJwqB4Nfe.png,w-150,lfo-top_right,l-end,l-end/medium_cafe_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:l-image,i-women-dress.jpeg,h-400,w-400,cm-pad_resize,bg-yellow,lfo-bottom_left,l-image,i-logo-white_SJwqB4Nfe.png,w-150,lfo-top_right,l-end,l-end/medium_cafe_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:l-image,i-women-dress.jpeg,h-400,w-400,cm-pad_resize,bg-yellow,lfo-bottom_left,l-image,i-logo-white_SJwqB4Nfe.png,w-150,lfo-top_right,l-end,l-end/medium_cafe_B1iTdD0C.jpg)

You can also apply additional transformations, such as `e-grayscale`, to modify the overlay image. Here's an example of turning the overlay image into a grayscale version.

URL - [https://ik.imagekit.io/demo/tr:l-image,i-women-dress.jpeg,h-400,w-400,e-grayscale,cm-pad_resize,bg-yellow,lfo-bottom_left,l-image,i-logo-white_SJwqB4Nfe.png,w-150,lfo-top_right,l-end,l-end/medium_cafe_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:l-image,i-women-dress.jpeg,h-400,w-400,e-grayscale,cm-pad_resize,bg-yellow,lfo-bottom_left,l-image,i-logo-white_SJwqB4Nfe.png,w-150,lfo-top_right,l-end,l-end/medium_cafe_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:l-image,i-women-dress.jpeg,h-400,w-400,e-grayscale,cm-pad_resize,bg-yellow,lfo-bottom_left,l-image,i-logo-white_SJwqB4Nfe.png,w-150,lfo-top_right,l-end,l-end/medium_cafe_B1iTdD0C.jpg)

## Text overlay

### Basic text overlay

You can add text overlays using the `l-text` parameter. By default, the text is displayed with the default font and a font size of 45. You can control the font size using the `fs` parameter.

URL - [https://ik.imagekit.io/demo/tr:l-text,i-overlay%20made%20easy,fs-45,l-end/medium_cafe_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:l-text,i-overlay%20made%20easy,fs-45,l-end/medium_cafe_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:l-text,i-overlay%20made%20easy,fs-45,l-end/medium_cafe_B1iTdD0C.jpg)

You can change the color of the text using the `co` parameter.

URL - [https://ik.imagekit.io/demo/tr:l-text,i-overlay%20made%20easy,co-red,fs-45,l-end/medium_cafe_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:l-text,i-overlay%20made%20easy,co-red,fs-45,l-end/medium_cafe_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:l-text,i-overlay%20made%20easy,co-red,fs-45,l-end/medium_cafe_B1iTdD0C.jpg)

You can also change the font using the `fo` parameter. Refer to the [list of supported fonts](/features/image-transformations/supported-text-font-list.md#in-built-fonts) for the available options. Apart from this, you can also use [custom fonts](#custom-fonts-example).

URL - [https://ik.imagekit.io/demo/tr:l-text,i-overlay%20made%20easy,ff-AbrilFatFace,co-yellow,fs-45,l-end/medium_cafe_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:l-text,i-overlay%20made%20easy,ff-AbrilFatFace,co-yellow,fs-45,l-end/medium_cafe_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:l-text,i-overlay%20made%20easy,ff-AbrilFatFace,co-yellow,fs-45,l-end/medium_cafe_B1iTdD0C.jpg)

### Adding colored background 

You can add a colored background to the text overlay using the `bg` parameter. For example, you can add a white background.

URL - [https://ik.imagekit.io/demo/tr:l-text,i-overlay%20made%20easy,fs-45,bg-white,l-end/medium_cafe_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:l-text,i-overlay%20made%20easy,fs-45,bg-white,l-end/medium_cafe_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:l-text,i-overlay%20made%20easy,fs-45,bg-white,l-end/medium_cafe_B1iTdD0C.jpg)

You can also control the transparency of the background by specifying an 8-digit RGBA hex code. The last two characters determine the opacity level (e.g., AAFF0040, 0f0fac75).

URL - [https://ik.imagekit.io/demo/tr:l-text,i-overlay%20made%20easy,fs-45,bg-FFFFFF50,l-end/medium_cafe_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:l-text,i-overlay%20made%20easy,fs-45,bg-FFFFFF50,l-end/medium_cafe_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:l-text,i-overlay%20made%20easy,fs-45,bg-FFFFFF50,l-end/medium_cafe_B1iTdD0C.jpg)

### Adding padding

You can add padding to the text overlay using the `pa` parameter. The padding can be controlled individually for each side using the CSS shorthand notation.

{% tabs %}
{% tab title="pa=10" %}
URL - [https://ik.imagekit.io/demo/tr:l-text,i-overlay%20made%20easy,fs-45,bg-white,pa-10,l-end/medium_cafe_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:l-text,i-overlay%20made%20easy,fs-45,bg-white,pa-10,l-end/medium_cafe_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:l-text,i-overlay%20made%20easy,fs-45,bg-white,pa-10,l-end/medium_cafe_B1iTdD0C.jpg)
{% endtab %}

{% tab title="pa=40 and w=300" %}
URL - [https://ik.imagekit.io/demo/tr:l-text,i-overlay%20made%20easy,fs-45,bg-white,pa-10,w-300,l-end/medium_cafe_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:l-text,i-overlay%20made%20easy,fs-45,bg-white,pa-10,w-300,l-end/medium_cafe_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:l-text,i-overlay%20made%20easy,fs-45,bg-white,pa-10,w-300,l-end/medium_cafe_B1iTdD0C.jpg)
{% endtab %}

{% tab title="pa=25_50_75_100" %}
URL - [https://ik.imagekit.io/demo/tr:l-text,i-overlay%20made%20easy,fs-45,bg-white,pa-25_50_75_100,l-end/medium_cafe_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:l-text,i-overlay%20made%20easy,fs-45,bg-white,pa-25_50_75_100,l-end/medium_cafe_B1iTdD0C.jpg)

* Top padding is 25
* Right padding is 50
* Bottom padding is 75
* Left padding is 100

![](https://ik.imagekit.io/demo/tr:l-text,i-overlay%20made%20easy,fs-45,bg-white,pa-25_50_75_100,l-end/medium_cafe_B1iTdD0C.jpg)
{% endtab %}

{% tab title="pa=25_75_60" %}
URL - [https://ik.imagekit.io/demo/tr:l-text,i-overlay%20made%20easy,fs-45,bg-white,pa-25_75_60,l-end/medium_cafe_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:l-text,i-overlay%20made%20easy,fs-45,bg-white,pa-25_75_60,l-end/medium_cafe_B1iTdD0C.jpg)

* Top padding is 25
* Right and left paddings are 75
* Bottom padding is 60

![](https://ik.imagekit.io/demo/tr:l-text,i-overlay%20made%20easy,fs-45,bg-white,pa-25_75_60,l-end/medium_cafe_B1iTdD0C.jpg)
{% endtab %}

{% tab title="otp=25_75" %}
URL - [https://ik.imagekit.io/demo/tr:l-text,i-overlay%20made%20easy,fs-45,bg-white,pa-25_75,l-end/medium_cafe_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:l-text,i-overlay%20made%20easy,fs-45,bg-white,pa-25_75,l-end/medium_cafe_B1iTdD0C.jpg)

* Top and bottom paddings are 25
* Right and left paddings are 75

![](https://ik.imagekit.io/demo/tr:l-text,i-overlay%20made%20easy,fs-45,bg-white,pa-25_75,l-end/medium_cafe_B1iTdD0C.jpg)
{% endtab %}
{% endtabs %}


### Custom fonts example

If you have custom font files, you can use them in text overlays. In this example, `Lacquer-Regular_nGqtVsBJT.ttf` is a custom font file uploaded to the root of the media library and accessible at the URL `https://ik.imagekit.io/demo/Lacquer-Regular_nGqtVsBJT.ttf`.

URL - [https://ik.imagekit.io/demo/tr:l-text,i-overlay%20made%20easy,ff-Lacquer-Regular_nGqtVsBJT.ttf,co-yellow,fs-45,l-end/medium_cafe_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:l-text,i-overlay%20made%20easy,ff-Lacquer-Regular_nGqtVsBJT.ttf,co-yellow,fs-45,l-end/medium_cafe_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:l-text,i-overlay%20made%20easy,ff-Lacquer-Regular_nGqtVsBJT.ttf,co-yellow,fs-45,l-end/medium_cafe_B1iTdD0C.jpg)

### Non-English character support

You can overlay text in any language with custom fonts, for example.

URL - [https://ik.imagekit.io/ikmedia/tr:w-600,h-600:l-text,i-इमेजकिट के साथ,ff-fonts@@Poppins-Regular_Q15GrYWmL.ttf,fs-42,ia-left,pa-10,ly-N100,bg-FFFFFF75,co-145FDD,l-end:l-text,i-दुनिया के कोने-कोने में लोगों तक पहुंचें,pa-5,ly-N35,lx-300,fs-28,ia-right,ff-fonts@@Poppins-Regular_Q15GrYWmL.ttf,bg-FFFFFF75,l-end/girl_in_white.jpg](https://ik.imagekit.io/ikmedia/tr:w-600,h-600:l-text,i-%E0%A4%87%E0%A4%AE%E0%A5%87%E0%A4%9C%E0%A4%95%E0%A4%BF%E0%A4%9F%20%E0%A4%95%E0%A5%87%20%E0%A4%B8%E0%A4%BE%E0%A4%A5,ff-fonts@@Poppins-Regular_Q15GrYWmL.ttf,fs-42,ia-left,pa-10,ly-N100,bg-FFFFFF75,co-145FDD,l-end:l-text,i-%E0%A4%A6%E0%A5%81%E0%A4%A8%E0%A4%BF%E0%A4%AF%E0%A4%BE%20%E0%A4%95%E0%A5%87%20%E0%A4%95%E0%A5%8B%E0%A4%A8%E0%A5%87-%E0%A4%95%E0%A5%8B%E0%A4%A8%E0%A5%87%20%E0%A4%AE%E0%A5%87%E0%A4%82%20%E0%A4%B2%E0%A5%8B%E0%A4%97%E0%A5%8B%E0%A4%82%20%E0%A4%A4%E0%A4%95%20%E0%A4%AA%E0%A4%B9%E0%A5%81%E0%A4%82%E0%A4%9A%E0%A5%87%E0%A4%82,pa-5,ly-N35,lx-300,fs-28,ia-right,ff-fonts@@Poppins-Regular_Q15GrYWmL.ttf,bg-FFFFFF75,l-end/girl_in_white.jpg)

![](https://ik.imagekit.io/ikmedia/tr:w-600,h-600:l-text,i-%E0%A4%87%E0%A4%AE%E0%A5%87%E0%A4%9C%E0%A4%95%E0%A4%BF%E0%A4%9F%20%E0%A4%95%E0%A5%87%20%E0%A4%B8%E0%A4%BE%E0%A4%A5,ff-fonts@@Poppins-Regular_Q15GrYWmL.ttf,fs-42,ia-left,pa-10,ly-N100,bg-FFFFFF75,co-145FDD,l-end:l-text,i-%E0%A4%A6%E0%A5%81%E0%A4%A8%E0%A4%BF%E0%A4%AF%E0%A4%BE%20%E0%A4%95%E0%A5%87%20%E0%A4%95%E0%A5%8B%E0%A4%A8%E0%A5%87-%E0%A4%95%E0%A5%8B%E0%A4%A8%E0%A5%87%20%E0%A4%AE%E0%A5%87%E0%A4%82%20%E0%A4%B2%E0%A5%8B%E0%A4%97%E0%A5%8B%E0%A4%82%20%E0%A4%A4%E0%A4%95%20%E0%A4%AA%E0%A4%B9%E0%A5%81%E0%A4%82%E0%A4%9A%E0%A5%87%E0%A4%82,pa-5,ly-N35,lx-300,fs-28,ia-right,ff-fonts@@Poppins-Regular_Q15GrYWmL.ttf,bg-FFFFFF75,l-end/girl_in_white.jpg)