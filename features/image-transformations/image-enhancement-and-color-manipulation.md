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
