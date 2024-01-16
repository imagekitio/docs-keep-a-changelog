# Image enhancement & color manipulation

## Image enhancement 

### Contrast stretch - (e-contrast)

It is used to automatically enhance the contrast of the image by using the full intensity values that particular image format allows. This means that the lighter sections of an image become even lighter and the darker sections becomes even brighter, thereby enhancing the contrast.

{% tabs %}
{% tab title="Original" %}
URL - [https://ik.imagekit.io/demo/tr:h-300/sample_image.jpg](https://ik.imagekit.io/demo/tr:h-300/sample_image.jpg)

![](https://ik.imagekit.io/demo/tr:h-300/sample_image.jpg)
{% endtab %}

{% tab title="With e-contrast" %}
URL - [https://ik.imagekit.io/demo/tr:h-300,e-contrast/sample_image.jpg](https://ik.imagekit.io/demo/tr:h-300,e-contrast/sample_image.jpg)

![](https://ik.imagekit.io/demo/tr:h-300,e-contrast/sample_image.jpg)
{% endtab %}
{% endtabs %}

### Sharpen - (e-sharpen)

It is used to sharpen the input image. It is useful when highlighting the edges and finer details within an image. 

Example usage - `e-sharpen-<number> `

If just the e-sharpen  parameter is used, then a default sharpening is performed on the input image. The default behaviour can be controlled by specifying a number that restricts the extent of sharpening performed.

{% tabs %}
{% tab title="No sharpening" %}
URL - [https://ik.imagekit.io/demo/tr:h-300/sample_image.jpg](https://ik.imagekit.io/demo/tr:h-300/sample_image.jpg)

![](https://ik.imagekit.io/demo/tr:h-300/sample_image.jpg)
{% endtab %}

{% tab title="Default e-sharpen" %}
URL - [https://ik.imagekit.io/demo/tr:e-sharpen,h-300/sample_image.jpg](https://ik.imagekit.io/demo/tr:e-sharpen,h-300/sample_image.jpg)

![](https://ik.imagekit.io/demo/tr:e-sharpen,h-300/sample_image.jpg)
{% endtab %}

{% tab title="e-sharpen=10" %}
URL - [https://ik.imagekit.io/demo/tr:e-sharpen-10,h-300/sample_image.jpg](https://ik.imagekit.io/demo/tr:e-sharpen-10,h-300/sample_image.jpg)

![](https://ik.imagekit.io/demo/tr:e-sharpen-10,h-300/sample_image.jpg)
{% endtab %}
{% endtabs %}

### Unsharp mask - (e-usm)

[Unsharp Masking (USM)](https://en.wikipedia.org/wiki/Unsharp_masking) is an image sharpening technique. This transform allows you to apply and control unsharp masks on your images. 

Example usage - `e-usm-<radius>-<sigma>-<amount>-<threshold>`

The amount of sharpening can be varied using 4 parameters - `radius` , `sigma` , `amount` , and `threshold`. This results in perceptually better images compared to using [e-sharpen](image-enhancement-and-color-manipulation.md#sharpen-e-sharpen). Only positive floating points are allowed for these 4 parameters.

{% tabs %}
{% tab title="Original image" %}
URL - [https://ik.imagekit.io/demo/tr:h-300/sample_image.jpg](https://ik.imagekit.io/demo/tr:h-300/sample_image.jpg)

![](https://ik.imagekit.io/demo/tr:h-300/sample_image.jpg)
{% endtab %}

{% tab title="radius=2, sigma=2, amount=0.8 and threshold=0.024" %}
URL - [https://ik.imagekit.io/demo/tr:h-300,e-usm-2-2-0.8-0.024/sample_image.jpg](https://ik.imagekit.io/demo/tr:h-300,e-usm-2-2-0.8-0.024/sample_image.jpg)

![](https://ik.imagekit.io/demo/tr:h-300,e-usm-2-2-0.8-0.024/sample_image.jpg)
{% endtab %}
{% endtabs %}

### Shadow - (e-shadow)

This feature adds a shadow under solid objects in an input image with a transparent background. 

The shadow is applied under the areas constituted by the non-transparent pixels in the input image. You can adjust the shadow's saturation, blur level, and positional offsets with the following parameters.

| Parameter             | Description                       | Range                            | Default value      |
|-----------------------|-----------------------------------|----------------------------------|--------------------|
| Blur (`bl`)           | Adjusts the blur level of the shadow. e.g. `e-shadow-bl-15`          | `0` to `15`  | `10` |
| Saturation (`st`)     | Specifies the saturation level of the shadow. e.g. `e-shadow-st-40`  | `0` to `100` | `30` |
| X Offset (`x`)        | Sets the horizontal offset of the shadow by x% of the image width. e.g. `e-shadow-x-10`. Supports negative values by prefixing `N`: e.g. `e-shadow-x-N10` | `0` to `100` or `N100` | `2` |
| Y Offset (`y`)        | Sets the vertical offset of the shadow by y% of the image height. e.g. `e-shadow-y-10`. Supports negative values by prefixing `N`: e.g. `e-shadow-y-N10` | `0` to `100` or `N100` | `2` |

Multiple parameters can be configured by appending them with an underscore (`_`). They will be applied independent of the order in which they are specified.

Example usage - `e-shadow-<blur>_<saturation>_<x-offset>_<y-offset>`, which would look like, `e-shadow-bl-15_st-40_x-10_y-N5`.

{% tabs %}
{% tab title="Default e-shadow" %}
URL - [https://ik.imagekit.io/demo/tr:e-shadow/lipstick_1M.png](https://ik.imagekit.io/demo/tr:e-shadow/lipstick_1M.png)

![](https://ik.imagekit.io/demo/tr:e-shadow/lipstick_1M.png)
{% endtab %}

{% tab title="blur=15, saturation=40, X offset=10 and Y offset=-5" %}
URL - [https://ik.imagekit.io/demo/tr:e-shadow-bl-15_st-40_x-10_y-N5/mirror_1M.png](https://ik.imagekit.io/demo/tr:e-shadow-bl-15_st-40_x-10_y-N5/mirror_1M.png)

![](https://ik.imagekit.io/demo/tr:e-shadow-bl-15_st-40_x-10_y-N5/mirror_1M.png)
{% endtab %}
{% endtabs %}

#### Limits

Shadows can only be applied to images with a resolution of maximum 2MP. Images with resolutions outside this limit will return a "400 - Bad Request" error.

To enable shadows on images larger than 2MP, you can resize the images either in the same chain or a chained transformation. 

e.g. For a 3MP image, `e-shadow` would not work. But `w-500,e-shadow` or `w-500:e-shadow` would work.

We have an input image with dimensions 2216x1500 (3MP). Applying only the shadow transformation will return an error with status code 400: `https://ik.imagekit.io/demo/tr:e-shadow/lipstick_2M.png`. 

Resize the image as follows to apply a shadow successfully:

{% tabs %}
{% tab title="Resize and shadow: w-500,e-shadow" %}
URL - [https://ik.imagekit.io/demo/tr:w-500,e-shadow/lipstick_2M.png](https://ik.imagekit.io/demo/tr:w-500,e-shadow/lipstick_2M.png)

![](https://ik.imagekit.io/demo/tr:w-500,e-shadow/lipstick_2M.png)
{% endtab %}

{% tab title="Chained resize then shadow: w-500:e-shadow" %}
URL - [https://ik.imagekit.io/demo/tr:w-500:e-shadow/lipstick_2M.png](https://ik.imagekit.io/demo/tr:w-500:e-shadow/lipstick_2M.png)

![](https://ik.imagekit.io/demo/tr:w-500:e-shadow/lipstick_2M.png)
{% endtab %}

{% tab title="Original 3MP image (2216x1500)" %}
URL - [https://ik.imagekit.io/demo/lipstick_2M.png](https://ik.imagekit.io/demo/lipstick_2M.png)

![](https://ik.imagekit.io/demo/lipstick_2M.png)
{% endtab %}

{% endtabs %}

### Gradient - (e-gradient)

This is used to add a gradient overlay over an input image.

The gradient formed is a *linear gradient* containing two colors, and it can be customised with the following parameters:

| Parameter       | Description       | Value Type        | Range      | Default Value     |
|-----------------|-------------------|-------------------|------------|-------------------|
| Linear Direction (`ld`) | **Optional**.<br/><br/>Sets the direction vector of the linear gradient.<br/><br/>For enhanced control over the direction, this parameter also accepts an integer in the range `0-359`, which specifies the angle (in degrees) when measured over a standard x-y axes with pivot passing through the geometrical centre of the image. For example, `0` would mean `top`, `45` would mean `top_right`, `90` would mean `right` etc. | `Number` or `String` | **Number** - `0` to `359`<br/><br/>**String** - `top`, `top_right`, `right`, `bottom_right`, `bottom`, `bottom_left`, `left`, `topleft` | `180` (or `bottom`) |
| From Color (`from`) | **Optional**.<br/><br/>This is used to specify a *from* color for the gradient.<br/><br/>RGBA Code (e.g. `FFAABB50`) is a color representation model where the first six characters must be the RGB Hex Code and the last two characters must be a number between `00` and `99`, which indicates the opacity level. For example, `00` represents an opacity level of `0.00`, `01` represents an opacity level of `0.01`, and so on. | `String` | An [SVG recognized color](https://www.w3.org/TR/SVG11/types.html#ColorKeywords) keyword name eg. `aqua`;<br/><br/>or a RGB Hex Code eg. `FF0000`;<br/><br/>or a RGBA Hex Code eg. `FF000040`; | `FFFFFF` |
| To Color (`to`) | **Optional**.<br/><br/>This is used to specify a *to* color for the gradient.<br/><br/>RGBA Code (e.g. `FFAABB50`) is a color representation model where the first six characters must be the RGB Hex Code and the last two characters must be a number between `00` and `99`, which indicates the opacity level. For example, `00` represents an opacity level of `0.00`, `01` represents an opacity level of `0.01`, and so on. | `String` | An [SVG recognized color](https://www.w3.org/TR/SVG11/types.html#ColorKeywords) keyword name eg. `aqua`;<br/><br/>or a RGB Hex Code eg. `FF0000`;<br/><br/>or a RGBA Hex Code eg. `FF000040`; | `00000000` |
| Stop Point (`sp`) | **Optional**.<br/><br/>This is used to dictate where the break point should be made between the `from` color and the `to` color. In simpler words, it is the point (along the direction vector of the gradient) at which the `to` color achieves it’s pure form i.e. no blending with the `from` color. <br/><br/>When the number is less than or equal to 1 i.e. a floating point, it would be treated as a percentage value. For example, `0.25` means 25%, `0.1` means 10% and so on.<br/><br/>When the number is greater than 1, it would be treated as an absolute value (in pixels). For example, `100` would mean 100px and so on. | `Number` or `String` | **Number** - A floating point number between `0.01` and `0.99` or an integer greater than or equal to `1`.<br/><br/>**String** - An arithmetic expression like `ih_div_2`. Read more about arithmetic expressions [here](../arithmetic-expressions-in-transformations.md). | `1` |

Multiple parameters can be configured by appending them with an underscore (`_`). They will be applied independent of the order in which they are specified.

Example usage - `e-gradient-<linear-direction>_<from-color>_<to-color>_<stop-point>`, which would look like, `e-gradient-ld-top_right_from-FF000040_to-00FF0040_sp-0.9`.

{% tabs %}
{% tab title="Original Image" %}
URL - [https://ik.imagekit.io/demo/tr:h-300/sample_image.jpg](https://ik.imagekit.io/demo/tr:h-300/sample_image.jpg)

![](https://ik.imagekit.io/demo/tr:h-300/sample_image.jpg)
{% endtab %}

{% tab title="Default e-gradient" %}
URL - [https://ik.imagekit.io/demo/tr:h-300,e-gradient/sample_image.jpg](https://ik.imagekit.io/demo/tr:h-300,e-gradient/sample_image.jpg)

![](https://ik.imagekit.io/demo/tr:h-300,e-gradient/sample_image.jpg)
{% endtab %}

{% tab title="linear-direction=45, from-color=FF000050, to-color=FFFFFF50, sp=0.5" %}
URL - [https://ik.imagekit.io/demo/tr:h-300,e-gradient-ld-45_from-FF000050_to-FFFFFF50_sp-0.5/sample_image.jpg](https://ik.imagekit.io/demo/tr:h-300,e-gradient-ld-45_from-FF000050_to-FFFFFF50_sp-0.5/sample_image.jpg)

![](https://ik.imagekit.io/demo/tr:h-300,e-gradient-ld-45_from-FF000050_to-FFFFFF50_sp-0.5/sample_image.jpg)
{% endtab %}

{% tab title="linear-direction=top, from-color=green, to-color=00FF0010, sp=1" %}
URL - [https://ik.imagekit.io/demo/tr:h-300,e-gradient-ld-top_from-green_to-00FF0010_sp-1/sample_image.jpg](https://ik.imagekit.io/demo/tr:h-300,e-gradient-ld-top_from-green_to-00FF0010_sp-1/sample_image.jpg)

![](https://ik.imagekit.io/demo/tr:h-300,e-gradient-ld-top_from-green_to-00FF0010_sp-1/sample_image.jpg)
{% endtab %}
{% endtabs %}

#### Gradient as a background

For input images containing a solid object over a transparent background, gradient can also be applied as an underlay.

##### Usage syntax

```markup
https://ik.imagekit.io/<imagekitId>/your-image.png?tr=e-gradient-from-red_to-white:l-image,i-your-image.png,t-false,l-end
```

##### Examples

{% tabs %}
{% tab title="Original Image" %}
URL - [https://ik.imagekit.io/demo/tr:w-500/lipstick_2M.png](https://ik.imagekit.io/demo/tr:w-500/lipstick_2M.png)

![](https://ik.imagekit.io/demo/tr:w-500/lipstick_2M.png)
{% endtab %}

{% tab title="Gradient as a background" %}
URL - [https://ik.imagekit.io/demo/tr:e-gradient-from-lightskyblue_to-mintcream:l-image,i-lipstick_2M.png,t-false,l-end:w-500/lipstick_2M.png](https://ik.imagekit.io/demo/tr:e-gradient-from-lightskyblue_to-mintcream:l-image,i-lipstick_2M.png,t-false,l-end:w-500/lipstick_2M.png)

![](https://ik.imagekit.io/demo/tr:e-gradient-from-lightskyblue_to-mintcream:l-image,i-lipstick_2M.png,t-false,l-end:w-500/lipstick_2M.png)
{% endtab %}
{% endtabs %}

{% hint style="info" %}
If you experience a positional displacement between the original image and the overlay image, use `t-false` to disable default trimming of the overlay layer like `l-image,i-your-image.png,t-false,l-end`.
{% endhint %}

#### Adding gradient blocks over images

You can also add a gradient block over an input image.

##### Usage syntax

```markup
https://ik.imagekit.io/<imagekitId>/your-image.png?tr=l-image,i-ik_canvas,e-gradient,l-end
```

You can also control the position of the gradient color overlay using these [positional parameters](./overlay-using-layers.md#position-of-layer).


##### Transformation of gradient block overlay

Following transformation parameters are supported on the gradient block overlay inside a layer.

| Parameter                                                                                | Description                        |
|------------------------------------------------------------------------------------------|------------------------------------|
| w                                                                                        | Width of solid color block. It can also accept arithmetic expressions such as `bw_div_2`, or `bw_mul_0.8`. Learn more about arithmetic expressions [here](../arithmetic-expressions-in-transformations.md). |
| h                                                                                        | Height of solid color block. It can also accept arithmetic expressions such as `bh_div_2`, or `bh_mul_0.8`. Learn more about arithmetic expressions [here](../arithmetic-expressions-in-transformations.md).  |
| [r](../image-transformations/resize-crop-and-other-transformations.md#radius-r)             | It is used to control the radius of the corner and accepts an integer value as the input. |

If you're looking to add a solid color block overlay instead, read more about it [here](./overlay-using-layers.md#add-solid-color-blocks-over-image).

##### Examples

{% tabs %}
{% tab title="Original Image" %}
URL - [https://ik.imagekit.io/demo/tr:w-500/lipstick_2M.png](https://ik.imagekit.io/demo/tr:w-500/lipstick_2M.png)

![](https://ik.imagekit.io/demo/tr:w-500/lipstick_2M.png)
{% endtab %}

{% tab title="Gradient block over the image" %}
URL - [https://ik.imagekit.io/demo/tr:l-image,i-ik_canvas,e-gradient-from-F2559A_to-F27855_ld-top,w-bw,h-bh_div_10,ly-N0,l-end:w-500/lipstick_2M.png](https://ik.imagekit.io/demo/tr:l-image,i-ik_canvas,e-gradient-from-F2559A_to-F27855_ld-top,w-bw,h-bh_div_10,ly-N0,l-end:w-500/lipstick_2M.png)

![](https://ik.imagekit.io/demo/tr:l-image,i-ik_canvas,e-gradient-from-F2559A_to-F27855_ld-top,w-bw,h-bh_div_10,ly-N0,l-end:w-500/lipstick_2M.png)
{% endtab %}
{% endtabs %}