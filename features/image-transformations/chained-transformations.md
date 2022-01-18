---
description: >-
  You can chain the output of one transformation as the input for the next to
  create the desired output image using chained transformations.
---

# Chained transformations

Some transformations are dependent on the sequence they're carried out in. Chained transformations provide a simple way to control the sequence in which transformations are applied.

Each step in the chained transform is separated by a colon `:`. The step appearing first is applied first.                      

{% tabs %}
{% tab title="Chained transformation" %}
```markup
      URL-endpoint               1st        2nd                                  
┌──────────────────────────┐┌────────────┐ ┌───┐
https://ik.imagekit.io/demo/tr:w-400,h-300:rt-90/default-image.jpg
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="Original image" %}
URL - [https://ik.imagekit.io/demo/default-image.jpg](https://ik.imagekit.io/demo/default-image.jpg)

![](https://ik.imagekit.io/demo/default-image.jpg)
{% endtab %}

{% tab title="Resize to 400x300 and then rotate 90°" %}
URL - [https://ik.imagekit.io/demo/tr:w-400,h-300:rt-90/default-image.jpg](https://ik.imagekit.io/demo/tr:w-400,h-300:rt-90/default-image.jpg)

![](https://ik.imagekit.io/demo/tr:w-400,h-300:rt-90/default-image.jpg)
{% endtab %}

{% tab title="Rotate 90° and then resize to 400x400" %}
URL - [https://ik.imagekit.io/demo/tr:rt-90:w-400,h-300/default-image.jpg](https://ik.imagekit.io/demo/tr:rt-90:w-400,h-300/default-image.jpg)

![](https://ik.imagekit.io/demo/tr:rt-90:w-400,h-300/default-image.jpg)
{% endtab %}
{% endtabs %}

Apart from providing more control on the output image, using chained transforms also makes it easier to logically understand the transformation sequence and have no surprises. Just by reading the transformation, you would be able to anticipate the output. The output of a chained transformation, compared to writing everything in the same transformation step, is much more predictable.

{% hint style="info" %}
Each transformation step is subject to [certain limits](../../limits-and-troubleshooting/limits.md#image-limits).
{% endhint %}

## Example

Let's say that we want an image to be first resized to 400x400, then we want to add a 100px padding to the left of that image which is black in color. Then we want to add ImageKit's logo in this padding on the left. The logo should have a height of 20px and should be positioned 10px from the left and 40px from the top.

This is a complex transformation to achieve. Let's break it down in to steps.

**1. Resize to 400x400**

This is fairly straightforward.

`w-400,h-400`

**2. Add a 100px padding to the left**

100px padding on the left means that the resulting image will have a width of 400+100px = 500px. And the image should be completely on the right in this image. This is achieved using the pad_resize crop mode (to pad the image in case resize doesn't match the aspect ratio) along with the focus parameter set to right (to focus the image to the right). We also need to set the background to black (#000000). The transformation thus becomes -

`w-500,h-400,cm-pad_resize,fo-right,bg-000000`

**3. Overlay ImageKit's logo**

ImageKit's logo is accessible at path `logo-white_SJwqB4Nfe.png` and then we can position and resize the overlay using overlay x, y and overlay height transforms. The transformation thus becomes

`oi-logo-white_SJwqB4Nfe.png,ox-10,oy-40,oh-20`

We combine these three steps into a chain with each step separated by a colon (":")

Final Chained Transformation

`w-400,h-400:w-500,h-400,cm-pad_resize,fo-right,bg-000000:oi-logo-white_SJwqB4Nfe.png,ox-10,oy-40,oh-20`

Final URL - [https://ik.imagekit.io/demo/img/default-image.jpg?tr=w-400,h-400:w-500,h-400,cm-pad_resize,fo-right,bg-000000:oi-logo-white_SJwqB4Nfe.png,ox-10,oy-50,oh-20](https://ik.imagekit.io/demo/img/default-image.jpg?tr=w-400,h-400:w-500,h-400,cm-pad_resize,fo-right,bg-000000:oi-logo-white_SJwqB4Nfe.png,ox-10,oy-50,oh-20)

![](https://ik.imagekit.io/demo/img/default-image.jpg?tr=w-400,h-400:w-500,h-400,cm-pad_resize,fo-right,bg-000000:oi-logo-white_SJwqB4Nfe.png,ox-10,oy-50,oh-20)
