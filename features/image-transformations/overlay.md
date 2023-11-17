---
description: >-
  A comprehensive guide that covers how you can overlay images and text over
  another image.
---

# Overlay (deprecated)


{% hint style="danger" %}
**Deprecation notice**\
This is old overlay syntax and will be deprecated on 31st Oct 2023. Starting 1st November 2023, this old syntax URL will start giving `400` bad request error. Please migrate to [new layer syntax](/features/image-transformations/overlay-using-layers.md) that supports overlay nesting, provides better positional control, and allows transformation at the layer level. You can start with [examples](/features/image-transformations/overlay-using-layers.md#examples) to learn quickly.
{% endhint %}

## Introduction about overlays

You can overlay [images](overlay.md#image-overlay) or [text](overlay.md#text-overlay) over other images for watermarking or creating a dynamic banner using custom text.

![Overlay logo over another image](https://ik.imagekit.io/demo/tr:oi-logo-white_SJwqB4Nfe.png/medium_cafe_B1iTdD0C.jpg)

The above image is created using below URL:

[https://ik.imagekit.io/demo/tr:oi-logo-white_SJwqB4Nfe.png/medium_cafe_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:oi-logo-white_SJwqB4Nfe.png/medium_cafe_B1iTdD0C.jpg)

Let's first understand what all is possible in overlay and then you can deep dive into different options available based on what you need. To help you understand, we have added many examples in this documentation.

### Position of overlay image

You can control the position of overlay image relative to base image using [overlay focus (ofo)](overlay.md#overlay-focus-ofo), [overlay X position (ox)](overlay.md#overlay-x-position-ox) or [overlay Y position (oy)](overlay.md#overlay-y-position-oy).

### Dimension of overlay image

You can control the dimension of overlay image using [overlay height (oh)](overlay.md#overlay-height-oh) and [overlay width (ow)](overlay.md#overlay-width-ow).

### Related blog post for creating photo collage

{% embed url="https://imagekit.io/blog/how-to-create-a-photo-collage/" %}



## Common overlay options

{% hint style="danger" %}
**Deprecation notice**\
This is old overlay syntax and will be deprecated on 31st Oct 2023. Starting 1st November 2023, this old syntax URL will start giving `400` bad request error. Please migrate to [new layer syntax](/features/image-transformations/overlay-using-layers.md) that supports overlay nesting, provides better positional control, and allows transformation at the layer level. You can start with [examples](/features/image-transformations/overlay-using-layers.md#examples) to learn quickly.
{% endhint %}

The following options are available for both [image](overlay.md#image-overlay) and [text](overlay.md#text-overlay) overlay.

### Overlay X position - (ox)

Used to provide granular control over the position of the overlay image relative to the base image.

The top left corner of the input image is considered as (0,0), and the bottom right corner as (w,h), where w is the width and h is the height of the input image. If `ox`  is 100, then the overlay image is placed at 100px from the left edge of the input image. If the `ox` value exceeds the dimensions of the input image, the values are adjusted and made equal to the corresponding dimension of the input image. Negative values are also supported with a leading N in front of the provided value. 

**Possible Values** include any positive integer. If prefixed with `N` , then treated as a negative integer.

{% tabs %}
{% tab title="X=35" %}
URL - [https://ik.imagekit.io/demo/tr:oi-logo-white_SJwqB4Nfe.png,ox-35/medium_cafe_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:oi-logo-white_SJwqB4Nfe.png,ox-35/medium_cafe_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:oi-logo-white_SJwqB4Nfe.png,ox-35/medium_cafe_B1iTdD0C.jpg)
{% endtab %}

{% tab title="X=N35 (Negative value)" %}
URL - [https://ik.imagekit.io/demo/tr:oi-logo-white_SJwqB4Nfe.png,ox-N35/medium_cafe_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:oi-logo-white_SJwqB4Nfe.png,ox-N35/medium_cafe_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:oi-logo-white_SJwqB4Nfe.png,ox-N35/medium_cafe_B1iTdD0C.jpg)
{% endtab %}
{% endtabs %}

### Overlay Y position - (oy)

Used to provide granular control over the position of the overlay image relative to the base image. 

The top left corner of the input image is considered as (0,0), and the bottom right corner as (w, h), where w is the width and h is the height of the input image.  If `oy`  is 150, then the overlay image is placed at 150px from the top edge of the input image. If the `oy` value exceeds the dimensions of the input image, the values are adjusted and made equal to the corresponding dimension of the input image. Negative values are also supported with a leading N in front of the provided value.

**Possible Values** include any positive integer. If prefixed with `N` , then treated as a negative integer.

{% tabs %}
{% tab title="Y=30" %}
URL - [https://ik.imagekit.io/demo/tr:oi-logo-white_SJwqB4Nfe.png,oy-30/medium_cafe_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:oi-logo-white_SJwqB4Nfe.png,oy-30/medium_cafe_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:oi-logo-white_SJwqB4Nfe.png,oy-30/medium_cafe_B1iTdD0C.jpg)
{% endtab %}

{% tab title="Y=N30 (Negative value)" %}
URL - [https://ik.imagekit.io/demo/tr:oi-logo-white_SJwqB4Nfe.png,oy-N30/medium_cafe_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:oi-logo-white_SJwqB4Nfe.png,oy-N30/medium_cafe_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:oi-logo-white_SJwqB4Nfe.png,oy-N30/medium_cafe_B1iTdD0C.jpg)
{% endtab %}

{% tab title="X=35 and Y=30" %}
URL - [https://ik.imagekit.io/demo/tr:oi-logo-white_SJwqB4Nfe.png,ox-35,oy-30/medium_cafe_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:oi-logo-white_SJwqB4Nfe.png,ox-35,oy-30/medium_cafe_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:oi-logo-white_SJwqB4Nfe.png,ox-35,oy-30/medium_cafe_B1iTdD0C.jpg)
{% endtab %}
{% endtabs %}

### Overlay height - (oh)

Used to specify the height of the overlay image.

{% hint style="info" %}
Using both [oh](overlay.md#overlay-height-oh) and [ow](overlay.md#overlay-width-ow) parameters together will crop the overlay image.
{% endhint %}

{% tabs %}
{% tab title="oh=20" %}
URL - [https://ik.imagekit.io/demo/tr:oi-logo-white_SJwqB4Nfe.png,oh-20/medium_cafe_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:oi-logo-white_SJwqB4Nfe.png,oh-20/medium_cafe_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:oi-logo-white_SJwqB4Nfe.png,oh-20/medium_cafe_B1iTdD0C.jpg)
{% endtab %}

{% tab title="oh=30" %}
URL - [https://ik.imagekit.io/demo/tr:oi-logo-white_SJwqB4Nfe.png,oh-30/medium_cafe_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:oi-logo-white_SJwqB4Nfe.png,oh-30/medium_cafe_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:oi-logo-white_SJwqB4Nfe.png,oh-30/medium_cafe_B1iTdD0C.jpg)
{% endtab %}
{% endtabs %}

### Overlay width - (ow)

Used to specify the width of the overlaid image.

{% hint style="info" %}
Using both [oh](overlay.md#overlay-height-oh) and [ow](overlay.md#overlay-width-ow) parameters together will crop the overlay image.
{% endhint %}

{% tabs %}
{% tab title="ow=50" %}
URL - [https://ik.imagekit.io/demo/tr:oi-logo-white_SJwqB4Nfe.png,ow-50/medium_cafe_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:oi-logo-white_SJwqB4Nfe.png,ow-50/medium_cafe_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:oi-logo-white_SJwqB4Nfe.png,ow-50/medium_cafe_B1iTdD0C.jpg)
{% endtab %}

{% tab title="ow=100" %}
URL - [https://ik.imagekit.io/demo/tr:oi-logo-white_SJwqB4Nfe.png,ow-100/medium_cafe_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:oi-logo-white_SJwqB4Nfe.png,ow-100/medium_cafe_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:oi-logo-white_SJwqB4Nfe.png,ow-100/medium_cafe_B1iTdD0C.jpg)
{% endtab %}
{% endtabs %}

### Overlay Background - (obg)

{% hint style="danger" %}
**Deprecation notice**\
This is old overlay syntax and will be deprecated on 31st Oct 2023. Starting 1st November 2023, this old syntax URL will start giving `400` bad request error. Please migrate to [new layer syntax](/features/image-transformations/overlay-using-layers.md) that supports overlay nesting, provides better positional control, and allows transformation at the layer level. You can start with [examples](/features/image-transformations/overlay-using-layers.md#examples) to learn quickly.
{% endhint %}

If you want to overlay a solid color block over a base image, use `obg` parameter.\
\
**Possible Values**
* RGB Hex code: A 6-digit hex code (eg. AAFF00, 0f0fac)
* RGBA Hex code: An 8-digit hex code. Last two characters must be a number between `00` and `99`, specifying the opacity level (eg. AAFF0040, 0f0fac75) 
* Color name: A standard color name in lowercase (eg. lightgreen, beige)

#### Overlay Background Transparency
This is supported via alpha component in RGB hex code which takes a numeric value between `00` and `99`, or via [oa](overlay.md#overlay-transparency-oa).

{% hint style="info" %}
If both [obg](overlay.md#overlay-background-obg) and [oa](overlay.md#overlay-transparency-oa) are set in a single transformation and [obg](overlay.md#overlay-background-obg) has an alpha component, then that value is used to set overlay background transparency. Otherwise, [oa](overlay.md#overlay-transparency-oa) value is used. If [obg](overlay.md#overlay-background-obg) is set to a standard color name (eg. `blue`), then the [oa](overlay.md#overlay-transparency-oa) value is ignored.
{% endhint %}

{% tabs %}
{% tab title="obg=00AAFF" %}
URL - [https://ik.imagekit.io/demo/tr:obg-00FFFF,oh-50,ow-600,ofo-bottom/medium_cafe_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:obg-00FFFF,oh-50,ow-600,ofo-bottom/medium_cafe_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:obg-00FFFF,oh-50,ow-600,ofo-bottom/medium_cafe_B1iTdD0C.jpg)
{% endtab %}

{% tab title="obg=00AAFF55 (55% opacity)" %}
URL - [https://ik.imagekit.io/demo/tr:obg-00FFFF55,oh-50,ow-600,ofo-bottom/medium_cafe_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:obg-00FFFF55,oh-50,ow-600,ofo-bottom/medium_cafe_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:obg-00FFFF55,oh-50,ow-600,ofo-bottom/medium_cafe_B1iTdD0C.jpg)
{% endtab %}
{% endtabs %}

### Overlay focus - (ofo)

{% hint style="danger" %}
**Deprecation notice**\
This is old overlay syntax and will be deprecated on 31st Oct 2023. Starting 1st November 2023, this old syntax URL will start giving `400` bad request error. Please migrate to [new layer syntax](/features/image-transformations/overlay-using-layers.md) that supports overlay nesting, provides better positional control, and allows transformation at the layer level. You can start with [examples](/features/image-transformations/overlay-using-layers.md#examples) to learn quickly.
{% endhint %}

You can control the relative position of overlay image using `ofo` parameter. This position is relative to base image.

Possible values include `center` , `top` , `left` , `bottom` , `right` , `top_left` , `top_right` , `bottom_left` , and `bottom_right` .\
\
**Default Value** - `center`

{% tabs %}
{% tab title="Default center" %}
URL - [https://ik.imagekit.io/demo/tr:oi-logo-white_SJwqB4Nfe.png/medium_cafe_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:oi-logo-white_SJwqB4Nfe.png/medium_cafe_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:oi-logo-white_SJwqB4Nfe.png/medium_cafe_B1iTdD0C.jpg)
{% endtab %}

{% tab title="Top" %}
URL - [https://ik.imagekit.io/demo/tr:oi-logo-white_SJwqB4Nfe.png,ofo-top/medium_cafe_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:oi-logo-white_SJwqB4Nfe.png,ofo-top/medium_cafe_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:oi-logo-white_SJwqB4Nfe.png,ofo-top/medium_cafe_B1iTdD0C.jpg)
{% endtab %}

{% tab title="Bottom left" %}
URL - [https://ik.imagekit.io/demo/tr:oi-logo-white_SJwqB4Nfe.png,ofo-bottom_left/medium_cafe_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:oi-logo-white_SJwqB4Nfe.png,ofo-bottom_left/medium_cafe_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:oi-logo-white_SJwqB4Nfe.png,ofo-bottom_left/medium_cafe_B1iTdD0C.jpg)
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="ow=50" %}
![](https://ik.imagekit.io/demo/tr:oi-logo-white_SJwqB4Nfe.png,ow-50/medium_cafe_B1iTdD0C.jpg)
{% endtab %}

{% tab title="ow=100" %}
URL - [https://ik.imagekit.io/demo/tr:oi-logo-white_SJwqB4Nfe.png,ow-100/medium_cafe_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:oi-logo-white_SJwqB4Nfe.png,ow-100/medium_cafe_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:oi-logo-white_SJwqB4Nfe.png,ow-100/medium_cafe_B1iTdD0C.jpg)
{% endtab %}
{% endtabs %}

## Image overlay

{% hint style="danger" %}
**Deprecation notice**\
This is old overlay syntax and will be deprecated on 31st Oct 2023. Starting 1st November 2023, this old syntax URL will start giving `400` bad request error. Please migrate to [new layer syntax](/features/image-transformations/overlay-using-layers.md) that supports overlay nesting, provides better positional control, and allows transformation at the layer level. You can start with [examples](/features/image-transformations/overlay-using-layers.md#examples) to learn quickly.
{% endhint %}

### Overlay image - (oi)

Used to overlay an image on top of another image (base image). This helps you generate watermarked images. 

Let's say base image URL is - [https://ik.imagekit.io/demo/medium_cafe_B1iTdD0C.jpg](https://ik.imagekit.io/demo/medium_cafe_B1iTdD0C.jpg)

Overlay image logo URL is - [https://ik.imagekit.io/demo/logo-white_SJwqB4Nfe.png](https://ik.imagekit.io/demo/logo-white_SJwqB4Nfe.png)

Now to overlay this logo over the base image we will pass the path of overlay image in `oi` parameter i.e. `logo-white_SJwqB4Nfe.png`

{% hint style="info" %}
1. ImageKit.io currently supports JPEG/JPG and PNG images for overlay.
2. The overlay image path should be accessible using your ImageKit.io account.
{% endhint %}

Final URL - \
[https://ik.imagekit.io/demo/tr:oi-logo-white_SJwqB4Nfe.png/medium_cafe_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:oi-logo-white_SJwqB4Nfe.png/medium_cafe_B1iTdD0C.jpg)

{% tabs %}
{% tab title="URL structure" %}
```markup
                        overlay image path        base image path                                    
                     ┌──────────────────────┐┌───────────────────────┐
{url-endpoint}/tr:oi-logo-white_SJwqB4Nfe.png/medium_cafe_B1iTdD0C.jpg
```
{% endtab %}
{% endtabs %}

![](https://ik.imagekit.io/demo/tr:oi-logo-white_SJwqB4Nfe.png/medium_cafe_B1iTdD0C.jpg)

#### Using overlay image that are stored in a nested folder

In the above example, overlay image logo was stored in home folder i.e. `/`, so there was no slash in the overlay image path. However it could be possible that you have overlay images stored within a specific folder, in that case there will be slashes (`/`) in overlay image path. To overcome this you need to replace slash `/` with `@@`.

For example, if the overlay image is at `https://ik.imagekit.io/demo/path/to/overlay.jpg`

Then it can be used as an overlay like below:

`https://ik.imagekit.io/demo/tr:oi-path@@to@@overlay.jpg/medium_cafe_B1iTdD0C.jpg`

#### Trimming of the overlay image

By default, ImageKit.io trims the overlay image before overlaying it on the base image. Trimming removes the similar colored pixels from the edges. By default, overlay image is trimmed.

There might be cases where you do not need such trimming to happen. Then, you can do that from the URL itself using the [overlay trim (oit)](overlay.md#overlay-trimming-oit) parameter and specifying it as `false`.

**Possible values** include `true` , `false` and integer values between `1`  and `99` that specify the threshold level for considering a particular pixel as "background".

For example, consider the image below, which has the same white logo to be overlaid but this time inside a black rectangular box.

Overlay image logo with black rectangular box - [https://ik.imagekit.io/demo/logo_white_black_bg.png](https://ik.imagekit.io/demo/logo_white_black_bg.png)

![](https://ik.imagekit.io/demo/logo_white_black_bg.png)

{% tabs %}
{% tab title="With trimming (default)" %}
URL - [https://ik.imagekit.io/demo/tr:oi-logo_white_black_bg.png/medium_cafe_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:oi-logo_white_black_bg.png/medium_cafe_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:oi-logo_white_black_bg.png/medium_cafe_B1iTdD0C.jpg)
{% endtab %}

{% tab title="Without trimming" %}
URL - [https://ik.imagekit.io/demo/tr:oi-logo_white_black_bg.png,oit-false/medium_cafe_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:oi-logo_white_black_bg.png,oit-false/medium_cafe_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:oi-logo_white_black_bg.png,oit-false/medium_cafe_B1iTdD0C.jpg)
{% endtab %}

{% tab title="Trim equal to 55" %}
URL - [https://ik.imagekit.io/ikmedia/tr:w-710,h-610:oi-women-dress-2.jpg,ox-0,oy-0,ow-600,oh-600,oib-5\_FFFFFF,oit-99:oi-women-dress-1.jpg,ox-500,oy-150,ow-200,oh-300,oib-5\_FFFFFF,oit-55/white-canvas.png](https://ik.imagekit.io/ikmedia/tr:w-710,h-610:oi-women-dress-2.jpg,ox-0,oy-0,ow-600,oh-600,oib-5\_FFFFFF,oit-99:oi-women-dress-1.jpg,ox-500,oy-150,ow-200,oh-300,oib-5\_FFFFFF,oit-55/white-canvas.png)

![](https://ik.imagekit.io/ikmedia/tr:w-710,h-610:oi-women-dress-2.jpg,ox-0,oy-0,ow-600,oh-600,oib-5\_FFFFFF,oit-99:oi-women-dress-1.jpg,ox-500,oy-150,ow-200,oh-300,oib-5\_FFFFFF,oit-55/white-canvas.png)
{% endtab %}

{% tab title="Trim equal to 80" %}
URL - [https://ik.imagekit.io/ikmedia/tr:w-710,h-610:oi-women-dress-2.jpg,ox-0,oy-0,ow-600,oh-600,oib-5\_FFFFFF,oit-99:oi-women-dress-1.jpg,ox-500,oy-150,ow-200,oh-300,oib-5\_FFFFFF,oit-80/white-canvas.png](https://ik.imagekit.io/ikmedia/tr:w-710,h-610:oi-women-dress-2.jpg,ox-0,oy-0,ow-600,oh-600,oib-5\_FFFFFF,oit-99:oi-women-dress-1.jpg,ox-500,oy-150,ow-200,oh-300,oib-5\_FFFFFF,oit-80/white-canvas.png)

![](https://ik.imagekit.io/ikmedia/tr:w-710,h-610:oi-women-dress-2.jpg,ox-0,oy-0,ow-600,oh-600,oib-5\_FFFFFF,oit-99:oi-women-dress-1.jpg,ox-500,oy-150,ow-200,oh-300,oib-5\_FFFFFF,oit-80/white-canvas.png)
{% endtab %}
{% endtabs %}



### **Overlay image aspect ratio (oiar)**

It is used to specify the aspect ratio of the overlay image or the ratio of width to height of the overlay image. 

This parameter must be used along with either the overlay height(oh) or overlay width(ow) parameter. The format for specifying this transformation is `oiar-<width>-<height>`.

If you specify both overlay height(oh) and overlay width(ow) in the URL along with this parameter, then the overlay image aspect ratio (oiar) is ignored.

{% tabs %}
{% tab title="oiar=4:3 & 3:4" %}
URL - [https://ik.imagekit.io/ikmedia/tr:w-715,h-460:oi-women-dress-2.jpg,ox-0,oy-0,ow-600,oib-5\_FFFFFF,oiar-4-3:oi-women-dress-1.jpg,ox-500,oy-100,ow-200,oiar-3-4,oib-5\_0F0F0F/white-canvas.png](https://ik.imagekit.io/ikmedia/tr:w-715,h-460:oi-women-dress-2.jpg,ox-0,oy-0,ow-600,oib-5\_FFFFFF,oiar-4-3:oi-women-dress-1.jpg,ox-500,oy-100,ow-200,oiar-3-4,oib-5\_0F0F0F/white-canvas.png)

![](https://ik.imagekit.io/ikmedia/tr:w-715,h-460:oi-women-dress-2.jpg,ox-0,oy-0,ow-600,oib-5\_FFFFFF,oiar-4-3:oi-women-dress-1.jpg,ox-500,oy-100,ow-200,oiar-3-4,oib-5\_0F0F0F/white-canvas.png)
{% endtab %}

{% tab title="oiar=4:3 & 9:16" %}
URL - [https://ik.imagekit.io/ikmedia/tr:w-715,h-460:oi-women-dress-2.jpg,ox-0,oy-0,ow-600,oib-5\_FFFFFF,oiar-4-3:oi-women-dress-1.jpg,ox-500,oy-50,ow-200,oiar-9-16,oib-5\_0F0F0F/white-canvas.png](https://ik.imagekit.io/ikmedia/tr:w-715,h-460:oi-women-dress-2.jpg,ox-0,oy-0,ow-600,oib-5\_FFFFFF,oiar-4-3:oi-women-dress-1.jpg,ox-500,oy-50,ow-200,oiar-9-16,oib-5\_0F0F0F/white-canvas.png)

![](https://ik.imagekit.io/ikmedia/tr:w-715,h-460:oi-women-dress-2.jpg,ox-0,oy-0,ow-600,oib-5\_FFFFFF,oiar-4-3:oi-women-dress-1.jpg,ox-500,oy-50,ow-200,oiar-9-16,oib-5\_0F0F0F/white-canvas.png)
{% endtab %}
{% endtabs %}

### **Overlay image background (oibg)**

It is used to specify the background color that must be used for the overlay image.

**Default Value** - Black 00000\
**Possible Values**
* RGB Hex code: A 6-digit hex code (eg. AAFF00, 0f0fac)
* RGBA Hex code: An 8-digit hex code. Last two characters must be a number between `00` and `99`, specifying the opacity level (eg. AAFF0040, 0f0fac75) 
* Color name: A standard color name in lowercase (eg. lightgreen, beige)

{% tabs %}
{% tab title="oibg=0F0F0F" %}
URL - [https://ik.imagekit.io/ikmedia/white-canvas.png?tr=w-666,h-1000:oi-women-dress-1.jpg,ox-0,oy-0,ow-666,oh-500,oicm-pad_resize,oifo-left,oibg-0F0F0F:oi-women-dress-4.jpg,ox-0,oy-500,ow-666,oh-500,oicm-pad_resize,oifo-right,oibg-0F0F0F:b-5\_0F0F0F](https://ik.imagekit.io/ikmedia/white-canvas.png?tr=w-666,h-1000:oi-women-dress-1.jpg,ox-0,oy-0,ow-666,oh-500,oicm-pad_resize,oifo-left,oibg-0F0F0F:oi-women-dress-4.jpg,ox-0,oy-500,ow-666,oh-500,oicm-pad_resize,oifo-right,oibg-0F0F0F:b-5\_0F0F0F)

![](https://ik.imagekit.io/ikmedia/white-canvas.png?tr=w-666,h-1000:oi-women-dress-1.jpg,ox-0,oy-0,ow-666,oh-500,oicm-pad_resize,oifo-left,oibg-0F0F0F:oi-women-dress-4.jpg,ox-0,oy-500,ow-666,oh-500,oicm-pad_resize,oifo-right,oibg-0F0F0F:b-5\_0F0F0F)
{% endtab %}
{% endtabs %}

### **Overlay image border (oib)**

This adds a border to the overlay image. It accepts two parameters - the width of the border and the color of the border.

Usage - `oib-<border-width>-<hex code>`

The width is specified as a number that is equivalent to the border width in pixels. The color code is specified as a 6-character hex code RRGGBB.

{% tabs %}
{% tab title="oib=10_FAD6A5 and 10_D73B3E" %}
URL - [https://ik.imagekit.io/ikmedia/tr:w-1240,h-620:oi-women-dress-2.jpg,ox-0,oy-0,ow-600,oh-600,oib-10\_FAD6A5:oi-women-dress-1.jpg,ox-620,oy-150,ow-600,oh-600,oib-10\_D73B3E/white-canvas.png](https://ik.imagekit.io/ikmedia/tr:w-1240,h-620:oi-women-dress-2.jpg,ox-0,oy-0,ow-600,oh-600,oib-10\_FAD6A5:oi-women-dress-1.jpg,ox-620,oy-150,ow-600,oh-600,oib-10\_D73B3E/white-canvas.png)

![](https://ik.imagekit.io/ikmedia/tr:w-1240,h-620:oi-women-dress-2.jpg,ox-0,oy-0,ow-600,oh-600,oib-10\_FAD6A5:oi-women-dress-1.jpg,ox-620,oy-150,ow-600,oh-600,oib-10\_D73B3E/white-canvas.png)
{% endtab %}

{% tab title="ob=5_FFFFFF" %}
URL - [https://ik.imagekit.io/ikmedia/tr:w-710,h-610:oi-women-dress-2.jpg,ox-0,oy-0,ow-600,oh-600,oib-5\_FFFFFF,oit-99:oi-women-dress-1.jpg,ox-500,oy-150,ow-200,oh-300,oib-5\_FFFFFF/white-canvas.png](https://ik.imagekit.io/ikmedia/tr:w-710,h-610:oi-women-dress-2.jpg,ox-0,oy-0,ow-600,oh-600,oib-5\_FFFFFF,oit-99:oi-women-dress-1.jpg,ox-500,oy-150,ow-200,oh-300,oib-5\_FFFFFF/white-canvas.png)

![](https://ik.imagekit.io/ikmedia/tr:w-710,h-610:oi-women-dress-2.jpg,ox-0,oy-0,ow-600,oh-600,oib-5\_FFFFFF,oit-99:oi-women-dress-1.jpg,ox-500,oy-150,ow-200,oh-300,oib-5\_FFFFFF/white-canvas.png)
{% endtab %}

{% tab title="ob=10_FFFFFF and 10_0F0F0F" %}
URL - [https://ik.imagekit.io/ikmedia/tr:w-730,h-620:oi-women-dress-2.jpg,ox-0,oy-0,ow-600,oh-600,oib-10\_FFFFFF,oit-99:oi-women-dress-1.jpg,ox-500,oy-150,ow-200,oh-300,oib-10\_0F0F0F/white-canvas.png](https://ik.imagekit.io/ikmedia/tr:w-730,h-620:oi-women-dress-2.jpg,ox-0,oy-0,ow-600,oh-600,oib-10\_FFFFFF,oit-99:oi-women-dress-1.jpg,ox-500,oy-150,ow-200,oh-300,oib-10\_0F0F0F/white-canvas.png)

![](https://ik.imagekit.io/ikmedia/tr:w-730,h-620:oi-women-dress-2.jpg,ox-0,oy-0,ow-600,oh-600,oib-10\_FFFFFF,oit-99:oi-women-dress-1.jpg,ox-500,oy-150,ow-200,oh-300,oib-10\_0F0F0F/white-canvas.png)
{% endtab %}
{% endtabs %}

### Overlay image DPR (oidpr)

{% hint style="danger" %}
**Deprecation notice**\
This is old overlay syntax and will be deprecated on 31st Oct 2023. Starting 1st November 2023, this old syntax URL will start giving `400` bad request error. Please migrate to [new layer syntax](/features/image-transformations/overlay-using-layers.md) that supports overlay nesting, provides better positional control, and allows transformation at the layer level. You can start with [examples](/features/image-transformations/overlay-using-layers.md#examples) to learn quickly.
{% endhint %}

It is used to specify the device pixel ratio that is used to calculate the dimensions of the overlay image. Extremely helpful when creating image transformations for devices with high device pixel ratio (DPR > 1), like the iPhone or high-end Android devices. 

The `oidpr` parameter can only be used when either the height or width of the desired output overlay image is specified. If the output image's height or width after considering the specified DPR  is less than 1px or greater than 5000px, the value of `oidpr` is not considered and the overlay height or width used in the URL is used. 

**Possible Values -** 0.1  to 5 .

{% tabs %}
{% tab title="oidpr=1.1" %}
URL - [https://ik.imagekit.io/ikmedia/tr:w-730,h-620:oi-women-dress-2.jpg,ox-0,oy-0,ow-600,oh-600,oib-10\_FFFFFF,oit-99:oi-women-dress-1.jpg,ox-500,oy-130,ow-200,oh-300,oib-10\_0F0F0F,oidpr-1.1/white-canvas.png](https://ik.imagekit.io/ikmedia/tr:w-730,h-620:oi-women-dress-2.jpg,ox-0,oy-0,ow-600,oh-600,oib-10\_FFFFFF,oit-99:oi-women-dress-1.jpg,ox-500,oy-130,ow-200,oh-300,oib-10\_0F0F0F,oidpr-1.1/white-canvas.png)

![](https://ik.imagekit.io/ikmedia/tr:w-730,h-620:oi-women-dress-2.jpg,ox-0,oy-0,ow-600,oh-600,oib-10\_FFFFFF,oit-99:oi-women-dress-1.jpg,ox-500,oy-130,ow-200,oh-300,oib-10\_0F0F0F,oidpr-1.1/white-canvas.png)
{% endtab %}
{% endtabs %}

### Overlay image quality (oiq)

It is used to specify the quality of the overlay image for lossy formats like JPEG and WebP.  A large quality number results in a bigger image file size with high overlay image quality. A small quality number results in a smaller image file size with lower overlay image quality. 

**Default Value -** 80 (can be managed from image settings in dashboard)

{% tabs %}
{% tab title="oiq=10 and 100" %}
URL - [https://ik.imagekit.io/ikmedia/tr:w-715,h-610:oi-women-dress-2.jpg,ox-0,oy-0,ow-600,oh-600,oib-5\_FFFFFF,oiq-10:oi-women-dress-1.jpg,ox-500,oy-150,ow-200,oh-300,oib-5\_0F0F0F,oiq-100/white-canvas.png](https://ik.imagekit.io/ikmedia/tr:w-715,h-610:oi-women-dress-2.jpg,ox-0,oy-0,ow-600,oh-600,oib-5\_FFFFFF,oiq-10:oi-women-dress-1.jpg,ox-500,oy-150,ow-200,oh-300,oib-5\_0F0F0F,oiq-100/white-canvas.png)

![](https://ik.imagekit.io/ikmedia/tr:w-715,h-610:oi-women-dress-2.jpg,ox-0,oy-0,ow-600,oh-600,oib-5\_FFFFFF,oiq-10:oi-women-dress-1.jpg,ox-500,oy-150,ow-200,oh-300,oib-5\_0F0F0F,oiq-100/white-canvas.png)
{% endtab %}
{% endtabs %}

### Overlay image cropping

{% hint style="danger" %}
**Deprecation notice**\
This is old overlay syntax and will be deprecated on 31st Oct 2023. Starting 1st November 2023, this old syntax URL will start giving `400` bad request error. Please migrate to [new layer syntax](/features/image-transformations/overlay-using-layers.md) that supports overlay nesting, provides better positional control, and allows transformation at the layer level. You can start with [examples](/features/image-transformations/overlay-using-layers.md#examples) to learn quickly.
{% endhint %}

Cropping in overlay behave the same way as [cropping works in ImageKit.io](resize-crop-and-other-transformations.md#crop-crop-modes-and-focus) in general. The only difference is that all cropping and focus related parameters are prefixed with `oi`.

So essentially:

* [c-at_least](resize-crop-and-other-transformations.md#min-size-cropping-strategy-c-at_least) becomes [oic-at_least](overlay.md#oic-at_least)
* [c-at_max](resize-crop-and-other-transformations.md#max-size-cropping-strategy-c-at_max) becomes [oic-at_max](overlay.md#oic-at_max)
* [c-force](resize-crop-and-other-transformations.md#forced-crop-strategy-c-force) becomes [oic-force](overlay.md#oic-force)
* [c-maintain_ratio](resize-crop-and-other-transformations.md#maintain-ratio-crop-strategy-c-maintain_ratio) becomes [oic-maintain_ratio](overlay.md#oic-maintain_ratio)
* [cm-extract](resize-crop-and-other-transformations.md#extract-crop-strategy-cm-extract) becomes [oicm-extract](overlay.md#oicm-extract)
* [cm-pad_extract](resize-crop-and-other-transformations.md#pad-extract-crop-strategy-cm-pad_extract) becomes [oicm-pad_extract](overlay.md#oicm-pad_extract)
* [cm-pad_resize](resize-crop-and-other-transformations.md#pad-resize-crop-strategy-cm-pad_resize) becomes [oicm-pad_resize](overlay.md#oicm-pad_resize)
* [fo](resize-crop-and-other-transformations.md#focus-fo) becomes [oifo](overlay.md#oifo)
* [fo-auto](resize-crop-and-other-transformations.md#auto-smart-cropping-fo-auto) becomes [oifo-auto](overlay.md#oifo-auto)
* [fo-custom](resize-crop-and-other-transformations.md#example-focus-using-custom-coordinates) becomes [oifo-custom](overlay.md#oifo-custom)
* [fo-face](resize-crop-and-other-transformations.md#face-cropping-fo-face) becomes [oifo-face](overlay.md#oifo-face)
* [x](resize-crop-and-other-transformations.md#examples-focus-using-cropped-image-coordinates) becomes [oix](overlay.md#oix)
* [y](resize-crop-and-other-transformations.md#examples-focus-using-cropped-image-coordinates) becomes [oiy](overlay.md#oiy)
* [xc](resize-crop-and-other-transformations.md#examples-focus-using-cropped-image-coordinates) becomes [oixc](overlay.md#oixc)
* [yc](resize-crop-and-other-transformations.md#examples-focus-using-cropped-image-coordinates) becomes [oiyc](overlay.md#oiyc)

#### oic-at_least

It works same as [c-at_least](resize-crop-and-other-transformations.md#min-size-cropping-strategy-c-at_least) but used in context of overlay images only.

#### oic-at_max

It works same as [c-at_max](resize-crop-and-other-transformations.md#max-size-cropping-strategy-c-at_max) but used in context of overlay images only.

#### oic-force

It works same as [c-force](resize-crop-and-other-transformations.md#forced-crop-strategy-c-force) but used in context of overlay images only.

#### oic-maintain_ratio

It works same as [c-maintain_ratio](resize-crop-and-other-transformations.md#maintain-ratio-crop-strategy-c-maintain_ratio) but used in context of overlay images only.

#### oicm-extract

It works same as [cm-extract](resize-crop-and-other-transformations.md#extract-crop-strategy-cm-extract) but used in context of overlay images only.

#### oicm-pad_extract

It works same as [cm-pad_extract](resize-crop-and-other-transformations.md#pad-extract-crop-strategy-cm-pad_extract) but used in context of overlay images only.

#### oicm-pad_resize

It works same as [cm-pad_resize](resize-crop-and-other-transformations.md#pad-resize-crop-strategy-cm-pad_resize) but used in context of overlay images only.

#### oifo

It works same as [fo](resize-crop-and-other-transformations.md#focus-fo) but used in context of overlay images only.

#### oifo-auto

It works same as [fo-auto](resize-crop-and-other-transformations.md#auto-smart-cropping-fo-auto) but used in context of overlay images only.

#### oifo-custom

It works same as [fo-custom](resize-crop-and-other-transformations.md#example-focus-using-custom-coordinates) but used in context of overlay images only.

#### oifo-face

It works same as [fo-face](resize-crop-and-other-transformations.md#face-cropping-fo-face) but used in context of overlay images only.

#### oix

It works same as [x](resize-crop-and-other-transformations.md#examples-focus-using-cropped-image-coordinates) but used in context of overlay images only.

#### oiy

It works same as [y](resize-crop-and-other-transformations.md#examples-focus-using-cropped-image-coordinates) but used in context of overlay images only.

#### oixc

It works same as [xc](resize-crop-and-other-transformations.md#examples-focus-using-cropped-image-coordinates) but used in context of overlay images only.

#### oiyc

It works same as [yc](resize-crop-and-other-transformations.md#examples-focus-using-cropped-image-coordinates) but used in context of overlay images only.

### Overlay trimming - (oit)

By default, ImageKit.io trims the overlay image before overlaying it on the base image. Trimming removes the similar colored pixels from the edges. There might be cases where you do not need such trimming to happen. Then, you can do that from the URL using `oit-false`.

This removes the transparency of the overlaid image.\
\
**Default Value** - `true`

Learn more from [example here](overlay.md#trimming-of-the-overlay-image).

## Text overlay

{% hint style="danger" %}
**Deprecation notice**\
This is old overlay syntax and will be deprecated on 31st Oct 2023. Starting 1st November 2023, this old syntax URL will start giving `400` bad request error. Please migrate to [new layer syntax](/features/image-transformations/overlay-using-layers.md) that supports overlay nesting, provides better positional control, and allows transformation at the layer level. You can start with [examples](/features/image-transformations/overlay-using-layers.md#examples) to learn quickly.
{% endhint %}

### Overlay text - (ot)

To overlay text on an image. Supported characters include all alphabets, numbers, spaces, \_, -, %, !, @, and &.

{% tabs %}
{% tab title="Base image" %}
URL - [https://ik.imagekit.io/demo/medium_cafe_B1iTdD0C.jpg](https://ik.imagekit.io/demo/medium_cafe_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/medium_cafe_B1iTdD0C.jpg)
{% endtab %}

{% tab title="Text overlay example" %}
URL - [https://ik.imagekit.io/demo/tr:ot-overlay%20made%20easy,ots-45/medium_cafe_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:ot-overlay%20made%20easy,ots-45/medium_cafe_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:ot-overlay%20made%20easy,ots-45/medium_cafe_B1iTdD0C.jpg)

Font size is set to 45 using [ots](overlay.md#overlay-text-size-ots) parameter.
{% endtab %}
{% endtabs %}

### Overlay text encoded - (ote)

It is similar to [overlay text (ot)](overlay.md#overlay-text-ot) but it allows you to overlay special unicode characters (e.g. ©, ®, ™ etc.) that you cannot otherwise pass in a URL as plain string. It accepts base64 string.

{% hint style="info" %}
1. The base64 string should be made URL safe to ensure that all padding characters(=) are included correctly. In Javascript, a function like `encodeURIComponent()` can be used for this.
2. Input above the length of 144 characters will be truncated.
{% endhint %}

Example** **URL** - **[https://ik.imagekit.io/demo/tr:ote-b3ZlcmxheSBtYWRlIGVhc3k%3D,ots-45/medium_cafe_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:ote-b3ZlcmxheSBtYWRlIGVhc3k%3D,ots-45/medium_cafe_B1iTdD0C.jpg)

Here `b3ZlcmxheSBtYWRlIGVhc3k%3D` is the base64 string for "overlay made easy"

![](https://ik.imagekit.io/demo/tr:ote-b3ZlcmxheSBtYWRlIGVhc3k%3D,ots-45/medium_cafe_B1iTdD0C.jpg)

### Overlay text width - (otw)

To specify the maximum width (in pixels) of the overlaid text on the image. The text is wrapped so that any words in a line that go beyond the given width are sent to the next line. Height of the text box is calculated automatically based on the total number of lines.

**Possible values** include any integer.

Example URL - [https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,ots-45,otc-00FFFF,otw-200/medium_cafe_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,ots-45,otc-00FFFF,otw-200/medium_cafe_B1iTdD0C.jpg)

{% tabs %}
{% tab title="otw=200" %}
URL -  [https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,ots-45,otc-00FFFF,otw-200/medium_cafe_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,ots-45,otc-00FFFF,otw-200/medium_cafe_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,ots-45,otc-00FFFF,otw-200/medium_cafe_B1iTdD0C.jpg)

Overlay text color is controlled using [otc](overlay.md#overlay-text-color-otc) parameter.
{% endtab %}

{% tab title="otw=400" %}
URL - [https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,ots-45,otc-00FFFF,otw-400/medium_cafe_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,ots-45,otc-00FFFF,otw-400/medium_cafe_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,ots-45,otc-00FFFF,otw-400/medium_cafe_B1iTdD0C.jpg)
{% endtab %}
{% endtabs %}

### Overlay text background - (otbg)

If you want to give the text block a background color, use this parameter.

**Possible Values**
* RGB Hex code: A 6-digit hex code (eg. AAFF00, 0f0fac)
* RGBA Hex code: An 8-digit hex code. Last two characters must be a number between `00` and `99`, specifying the opacity level (eg. AAFF0040, 0f0fac75) 
* Color name: A standard color name in lowercase (eg. lightgreen, beige)

#### Overlay Text Background Transparency
This is supported via alpha component in RGB hex code which takes a numeric value between `00` and `99`, or via [oa](overlay.md#overlay-transparency-oa).

{% hint style="info" %}
If both [otbg](overlay.md#overlay-text-background-otbg) and [oa](overlay.md#overlay-transparency-oa) are set in a single transformation and [otbg](overlay.md#overlay-text-background-otbg) has an alpha component, then this value is used to set overlay text transparency. Otherwise, [oa](overlay.md#overlay-transparency-oa) value is used. If [otbg](overlay.md#overlay-text-background-otbg) is set to a standard color name (eg. `blue`), then the [oa](overlay.md#overlay-transparency-oa) value is ignored.
{% endhint %}

{% tabs %}
{% tab title="With solid color background" %}
URL - [https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,ots-45,otc-000000,otbg-FFFFFF/medium_cafe_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,ots-45,otc-000000,otbg-FFFFFF/medium_cafe_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,ots-45,otc-000000,otbg-FFFFFF/medium_cafe_B1iTdD0C.jpg)
{% endtab %}

{% tab title="Solid color background with transparency" %}
URL - [https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,ots-45,otc-000000,otbg-FFFFFF75/medium_cafe_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,ots-45,otc-000000,otbg-FFFFFF75/medium_cafe_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,ots-45,otc-000000,otbg-FFFFFF75/medium_cafe_B1iTdD0C.jpg)
{% endtab %}
{% endtabs %}

### Overlay text padding - (otp)

{% hint style="danger" %}
**Deprecation notice**\
This is old overlay syntax and will be deprecated on 31st Oct 2023. Starting 1st November 2023, this old syntax URL will start giving `400` bad request error. Please migrate to [new layer syntax](/features/image-transformations/overlay-using-layers.md) that supports overlay nesting, provides better positional control, and allows transformation at the layer level. You can start with [examples](/features/image-transformations/overlay-using-layers.md#examples) to learn quickly.
{% endhint %}

To specify the padding around the overlaid text on the image.

**Possible values** include any positive integer or a set of positive integers separated by underscores. 

The set of integers follow CSS shorthand order for determining the padding along each side of the overlay. Learn from the examples:

{% tabs %}
{% tab title="otp=40" %}
URL - [https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,ots-45,otc-000000,otbg-FFFFFF80,otp-40/medium_cafe_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,ots-45,otc-000000,otbg-FFFFFF80,otp-40/medium_cafe_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,ots-45,otc-000000,otbg-FFFFFF80,otp-40/medium_cafe_B1iTdD0C.jpg)
{% endtab %}

{% tab title="otp=40 and otw=300" %}
URL - [https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,ots-45,otc-000000,otw-300,otbg-FFFFFF80,otp-40/medium_cafe_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,ots-45,otc-000000,otw-300,otbg-FFFFFF80,otp-40/medium_cafe_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,ots-45,otc-000000,otw-300,otbg-FFFFFF80,otp-40/medium_cafe_B1iTdD0C.jpg)
{% endtab %}

{% tab title="otp=25_50_75_100" %}
URL - [https://ik.imagekit.io/demo/tr:ot-overlay%20made%20easy,ots-45,otc-000000,otbg-FFFFFF80,otp-25\_50\_75\_100/medium_cafe_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:ot-overlay%20made%20easy,ots-45,otc-000000,otbg-FFFFFF80,otp-25\_50\_75\_100/medium_cafe_B1iTdD0C.jpg)

* Top padding is 25
* Right padding is 50
* Bottom padding is 75
* Left padding is 100

![](https://ik.imagekit.io/demo/tr:ot-overlay%20made%20easy,ots-45,otc-000000,otbg-FFFFFF80,otp-25\_50\_75\_100/medium_cafe_B1iTdD0C.jpg)
{% endtab %}

{% tab title="otp=25_75_60" %}
URL - [https://ik.imagekit.io/demo/tr:ot-overlay%20made%20easy,ots-45,otc-000000,otbg-FFFFFF80,otp-25\_75\_60/medium_cafe_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:ot-overlay%20made%20easy,ots-45,otc-000000,otbg-FFFFFF80,otp-25\_75\_60/medium_cafe_B1iTdD0C.jpg)

* Top padding is 25
* Right and left paddings are 75
* Bottom padding is 60

![](https://ik.imagekit.io/demo/tr:ot-overlay%20made%20easy,ots-45,otc-000000,otbg-FFFFFF80,otp-25\_75\_60/medium_cafe_B1iTdD0C.jpg)
{% endtab %}

{% tab title="otp=25_75" %}
URL - [https://ik.imagekit.io/demo/tr:ot-overlay%20made%20easy,ots-45,otc-000000,otbg-FFFFFF80,otp-25\_75/medium_cafe_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:ot-overlay%20made%20easy,ots-45,otc-000000,otbg-FFFFFF80,otp-25\_75/medium_cafe_B1iTdD0C.jpg)

* Top and bottom paddings are 25
* Right and left paddings are 75

![](https://ik.imagekit.io/demo/tr:ot-overlay%20made%20easy,ots-45,otc-000000,otbg-FFFFFF80,otp-25\_75/medium_cafe_B1iTdD0C.jpg)
{% endtab %}
{% endtabs %}

### Overlay text inner alignment - (otia)

To specify the alignment of the overlaid text on the image when text is wrapped with overlay text width (otw).

**Possible Values** include `left`, `right` and `center` (default). 

{% tabs %}
{% tab title="Left align" %}
URL - [https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,ots-45,otc-000000,otw-300,otbg-FFFFFF80,otp-40,otia-left/medium_cafe_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,ots-45,otc-000000,otw-300,otbg-FFFFFF80,otp-40,otia-left/medium_cafe_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,ots-45,otc-000000,otw-300,otbg-FFFFFF80,otp-40,otia-left/medium_cafe_B1iTdD0C.jpg)
{% endtab %}

{% tab title="Right align" %}
URL - [https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,ots-45,otc-000000,otw-300,otbg-FFFFFF80,otp-40,otia-right/medium_cafe_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,ots-45,otc-000000,otw-300,otbg-FFFFFF80,otp-40,otia-right/medium_cafe_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,ots-45,otc-000000,otw-300,otbg-FFFFFF80,otp-40,otia-right/medium_cafe_B1iTdD0C.jpg)
{% endtab %}
{% endtabs %}

### Overlay text color - (otc)

{% hint style="danger" %}
**Deprecation notice**\
This is old overlay syntax and will be deprecated on 31st Oct 2023. Starting 1st November 2023, this old syntax URL will start giving `400` bad request error. Please migrate to [new layer syntax](/features/image-transformations/overlay-using-layers.md) that supports overlay nesting, provides better positional control, and allows transformation at the layer level. You can start with [examples](/features/image-transformations/overlay-using-layers.md#examples) to learn quickly.
{% endhint %}

To specify the color and transparency of the overlaid text on the image. \
\
**Possible Values**
* RGB Hex code: A 6-digit hex code (eg. AAFF00, 0f0fac)
* RGBA Hex code: An 8-digit hex code. Last two characters must be a number between `00` and `99`, specifying the opacity level (eg. AAFF0040, 0f0fac75) 
* Color name: A standard color name in lowercase (eg. lightgreen, beige)

#### Overlay Text Transparency
This is supported via alpha component in RGB hex code which takes a numeric value between 00 and 99, or via [oa](overlay.md#overlay-transparency-oa).

{% hint style="info" %}
If both [otc](overlay.md#overlay-text-color-otc) and [oa](overlay.md#overlay-transparency-oa) are set in a single transformation and [otc](overlay.md#overlay-text-color-otc) has an alpha component, then this value is used to set overlay text transparency. Otherwise, [oa](overlay.md#overlay-transparency-oa) value is used. If [otc](overlay.md#overlay-text-color-otc) is set to a standard color name (eg. `blue`), then the [oa](overlay.md#overlay-transparency-oa) value is ignored.
{% endhint %}

{% tabs %}
{% tab title="otc=00AAFF (no transparancy)" %}
URL - [https://ik.imagekit.io/demo/tr:ot-overlay%20made%20easy,ots-45,otc-00FFFF/medium_cafe_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:ot-overlay%20made%20easy,ots-45,otc-00FFFF/medium_cafe_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:ot-overlay%20made%20easy,ots-45,otc-00FFFF/medium_cafe_B1iTdD0C.jpg)
{% endtab %}

{% tab title="otc=00FFFF55 (55% opacity)" %}
URL - [https://ik.imagekit.io/demo/tr:ot-overlay%20made%20easy,ots-45,otc-00FFFF55/medium_cafe_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:ot-overlay%20made%20easy,ots-45,otc-00FFFF55/medium_cafe_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:ot-overlay%20made%20easy,ots-45,otc-00FFFF55/medium_cafe_B1iTdD0C.jpg)
{% endtab %}
{% endtabs %}

### Overlay text font - (otf)

To specify the font of the overlaid text on the image. You can choose [any font from this list](supported-text-font-list.md#in-built-fonts) or use a [custom font](overlay.md#using-custom-fonts). 

{% tabs %}
{% tab title="otf = Open Sans" %}
URL - [https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,ots-45,otc-00FFFF,otf-Open%20Sans/medium_cafe_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,ots-45,otc-00FFFF,otf-Open%20Sans/medium_cafe_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,ots-45,otc-00FFFF,otf-Open%20Sans/medium_cafe_B1iTdD0C.jpg)
{% endtab %}

{% tab title="otf = Roboto" %}
URL - [https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,ots-45,otc-00FFFF,otf-Roboto/medium_cafe_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,ots-45,otc-00FFFF,otf-Roboto/medium_cafe_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,ots-45,otc-00FFFF,otf-Roboto/medium_cafe_B1iTdD0C.jpg)
{% endtab %}
{% endtabs %}

#### Using custom fonts

You can use a custom font in text overlay. Upload the font file in your media library and pass the path in `otf` parameter.

{% tabs %}
{% tab title="Text overlay using custom font" %}
URL - [https://ik.imagekit.io/demo/tr:otf-Lacquer-Regular_nGqtVsBJT.ttf,ot-Hello%20World,ots-72,otc-FFF000/default-image.jpg](https://ik.imagekit.io/demo/tr:otf-Lacquer-Regular_nGqtVsBJT.ttf,ot-Hello%20World,ots-72,otc-FFF000/default-image.jpg)

![](https://ik.imagekit.io/demo/tr:otf-Lacquer-Regular_nGqtVsBJT.ttf,ot-Hello%20World,ots-72,otc-FFF000/default-image.jpg)

**Note:**\
****Here the font-file is available at path `/Lacquer-Regular_nGqtVsBJT.ttf` in media library. You will need to pass the whole path including extension.
{% endtab %}
{% endtabs %}

### Overlay text size - (ots)

To specify the size of the overlaid text on the image. \
\
**Possible Values** include any integer.

{% tabs %}
{% tab title="ots=45" %}
URL - [https://ik.imagekit.io/demo/tr:ot-overlays made easy,ots-45,otc-00FFFF/medium_cafe_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,ots-45,otc-00FFFF/medium_cafe_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,ots-45,otc-00FFFF/medium_cafe_B1iTdD0C.jpg)
{% endtab %}

{% tab title="ots=60" %}
URL - [https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,ots-60,otc-00FFFF/medium_cafe_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,ots-60,otc-00FFFF/medium_cafe_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,ots-60,otc-00FFFF/medium_cafe_B1iTdD0C.jpg)
{% endtab %}
{% endtabs %}

### Overlay text typography - (ott)

To specify the typography of the font used for the overlaid image. 

**Possible Values** include bold `b`  and italics `i`.

{% tabs %}
{% tab title="Bold" %}
URL - [https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,ots-45,ott-b,otc-00FFFF/medium_cafe_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,ots-45,ott-b,otc-00FFFF/medium_cafe_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,ots-45,ott-b,otc-00FFFF/medium_cafe_B1iTdD0C.jpg)
{% endtab %}

{% tab title="Italics" %}
URL - [https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,ots-45,ott-i,otc-00FFFF/medium_cafe_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,ots-45,ott-i,otc-00FFFF/medium_cafe_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,ots-45,ott-i,otc-00FFFF/medium_cafe_B1iTdD0C.jpg)
{% endtab %}
{% endtabs %}

### Overlay radius - (or)

To add rounded corners to an overlay background, or to print a circular overlay, use this parameter.

**Possible Values** include any positive integer.

**Note: **Radius cannot exceed half the length of the shorter side of the overlay element. ‌

{% tabs %}
{% tab title="or=30 & otbg=FFFFFF80" %}
URL - [https://ik.imagekit.io/demo/tr:ot-overlay%20made%20easy,or-30,otbg-FFFFFF80,ots-45,otc-000000,otp-40/medium_cafe_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:ot-overlay%20made%20easy,or-30,otbg-FFFFFF80,ots-45,otc-000000,otp-40/medium_cafe_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:ot-overlay%20made%20easy,or-30,otbg-FFFFFF80,ots-45,otc-000000,otp-40/medium_cafe_B1iTdD0C.jpg)
{% endtab %}

{% tab title="Circle (or=150 & oh,ow=300)" %}
URL -** **[https://ik.imagekit.io/demo/tr:obg-FFFFFF80,or-150,oh-300,ow-300,ofo-center/medium_cafe_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:obg-FFFFFF80,or-150,oh-300,ow-300,ofo-center/medium_cafe_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:obg-FFFFFF80,or-150,oh-300,ow-300,ofo-center/medium_cafe_B1iTdD0C.jpg)
{% endtab %}

{% tab title="or=40 & obg=00FFFF80" %}
URL - [https://ik.imagekit.io/demo/tr:obg-00FFFF80,or-40,oh-80,ow-400,ofo-bottom/medium_cafe_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:obg-00FFFF80,or-40,oh-80,ow-400,ofo-bottom/medium_cafe_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:obg-00FFFF80,or-40,oh-80,ow-400,ofo-bottom/medium_cafe_B1iTdD0C.jpg)
{% endtab %}
{% endtabs %}

### Overlay transparency - (oa)

To specify the transparency level of the overlaid image. \
\
**Possible Values** include integers from `1`  to `9` .\
\
**Note:** This transform is applied only if the corresponding color transform ([obg](overlay.md#overlay-background-obg), [otc](overlay.md#overlay-text-color-otc), etc.) is specified as a Hex code and does not have an alpha component.

{% tabs %}
{% tab title="oa=5" %}
URL - [https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,oa-5,ots-45/medium_cafe_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,oa-5,ots-45/medium_cafe_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,oa-5,ots-45/medium_cafe_B1iTdD0C.jpg)
{% endtab %}

{% tab title="oa=2" %}
URL - [https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,oa-2,ots-45/medium_cafe_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,oa-2,ots-45/medium_cafe_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,oa-2,ots-45/medium_cafe_B1iTdD0C.jpg)
{% endtab %}
{% endtabs %}
