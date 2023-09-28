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

This feature adds a shadow under solid objects in an input image with a transparent background. The shadow is added under the areas constituted by the non-transparent pixels, according to the following parameters.

| Parameter             | Description                       | Range                            | Default value      |
|-----------------------|-----------------------------------|----------------------------------|--------------------|
| Blur (`bl`)           | Adjusts the blur level of the shadow. e.g. `e-shadow-bl-15`          | `0` to `15`  | `10` |
| Saturation (`st`)     | Specifies the saturation level of the shadow. e.g. `e-shadow-st-40`  | `0` to `100` | `30` |
| X Offset (`x`)        | Sets the horizontal offset of the shadow by x% of the image width. e.g. `e-shadow-x-10`. Supports negative values by prefixing `N`: e.g. `e-shadow-x-N10` | `0` to `100` or `N100` | `2` |
| Y Offset (`y`)        | Sets the vertical offset of the shadow by y% of the image width. e.g. `e-shadow-y-10`. Supports negative values by prefixing `N`: e.g. `e-shadow-y-N10` | `0` to `100` or `N100` | `2` |

Multiple parameters can be configured by appending them with an underscore (`_`). They will be applied independent of the order in which they are specified.

Example usage - `e-shadow-<blur>_<saturation>_<x-offset>_<y-offset>`, which would look like, `e-shadow-bl-15_st-40_x-10_y-N5`.

{% tabs %}
{% tab title="Original image" %}
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

For example, for a 3MP image, `e-shadow` would not work. But `w-200,e-shadow` or `w-200:e-shadow` would work. See examples below:

For an input image with dimensions 2216x1500 (3MP), the following shadow transformation will return an error with status code 400: `https://ik.imagekit.io/demo/tr:e-shadow/lipstick_2M.png`. 

Resize the image as follows to apply a shadow successfully:

{% tabs %}
{% tab title="Resize and shadow: w-500,e-shadow" %}
URL - [https://ik.imagekit.io/demo/tr:w-500,e-shadow/lipstick_2M.png](https://ik.imagekit.io/demo/tr:w-500,e-shadow/lipstick_2M.png)

![](https://ik.imagekit.io/demo/tr:w-500,e-shadow/lipstick_2M.png)
{% endtab %}

{% tab title=Chained resize then shadow: w-500:e-shadow" %}
URL - [https://ik.imagekit.io/demo/tr:w-500:e-shadow/lipstick_2M.png](https://ik.imagekit.io/demo/tr:w-500:e-shadow/lipstick_2M.png)

![](https://ik.imagekit.io/demo/tr:w-500:e-shadow/lipstick_2M.png)
{% endtab %}

{% endtabs %}
