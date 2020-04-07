---
description: >-
  A comprehensive guide that covers how you can overlay images and text over
  another image.
---

# Overlay

## Image overlay

### Overlay image - \(oi\)

Used to overlay an image on top of another image \(base image\). This helps you generate watermarked images. 

Let's say base image URL is - [https://ik.imagekit.io/demo/medium\_cafe\_B1iTdD0C.jpg](https://ik.imagekit.io/demo/medium_cafe_B1iTdD0C.jpg)

Overlay image logo URL is - [https://ik.imagekit.io/demo/logo-white\_SJwqB4Nfe.png](https://ik.imagekit.io/demo/logo-white_SJwqB4Nfe.png)

Now to overlay this logo over the base image we will pass the path of overlay image in `oi` parameter i.e. `logo-white_SJwqB4Nfe.png`

{% hint style="info" %}
1. ImageKit.io currently supports JPEG/JPG and PNG images for overlay.
2. The overlay image path should be accessible using your ImageKit.io account.
{% endhint %}

Final URL -   
[https://ik.imagekit.io/demo/tr:oi-logo-white\_SJwqB4Nfe.png/medium\_cafe\_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:oi-logo-white_SJwqB4Nfe.png/medium_cafe_B1iTdD0C.jpg)

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

In the above example, overlay image logo was stored in home folder i.e. `/`, so there was no slash in the overlay image path. However it could be possible that you have overlay images stored within a specific folder, in that case there will be slashes \(`/`\) in overlay image path. To overcome this you need to replace slash `/` with `@@`.

For example, if the overlay image is at `https://ik.imagekit.io/demo/path/to/overlay.jpg`

Then it can be used as an overlay like below:

`https://ik.imagekit.io/demo/tr:oi-path@@to@@overlay.jpg/medium_cafe_B1iTdD0C.jpg`

#### Controlling the position of overlay image

You can control the position of overlay image using [overlay focus \(ofo\)](overlay.md#overlay-focus-ofo), [overlay X position \(ox\)](overlay.md#overlay-x-position-ox) or [overlay Y position \(oy\)](overlay.md#overlay-y-position-oy).

#### Controlling the dimension of overlay image

You can control the position of overlay image using over height \(oh\) and overlay width \(ow\).

#### Trimming of the overlay image

By default, ImageKit.io trims the overlay image before overlaying it on the base image. There might be cases where you do not need such trimming to happen. Then, you can do that from the URL itself using the overlay trim \(oit\) parameter and specifying it as `false`.

For example, consider the image below, which has the same white logo to be overlaid but this time inside a black rectangular box.

Overlay image logo with black rectangular box - [https://ik.imagekit.io/demo/logo\_white\_black\_bg.png](https://ik.imagekit.io/demo/logo_white_black_bg.png)

![](https://ik.imagekit.io/demo/logo_white_black_bg.png)

{% tabs %}
{% tab title="With trimming \(default\)" %}
URL - [https://ik.imagekit.io/demo/tr:oi-logo\_white\_black\_bg.png/medium\_cafe\_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:oi-logo_white_black_bg.png/medium_cafe_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:oi-logo_white_black_bg.png/medium_cafe_B1iTdD0C.jpg)
{% endtab %}

{% tab title="Without trimming" %}
URL - [https://ik.imagekit.io/demo/tr:oi-logo\_white\_black\_bg.png,oit-false/medium\_cafe\_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:oi-logo_white_black_bg.png,oit-false/medium_cafe_B1iTdD0C.jpg)
{% endtab %}
{% endtabs %}

### Overlay Background - \(obg\)

If you want to overlay a solid color block over a base image, use `obg` parameter.  
  
**Possible Values**- Valid RGB Hex Code with optional alpha component

For example - `00AAFF` \(solid color\) or `00AAFF55` \(with 55% opacity\)

**Overlay Background Transparency** is supported via alpha component in RGB hex code which takes a numeric value between `00` and `99`.

{% hint style="info" %}
If both [obg](overlay.md#overlay-background-obg) and [oa](overlay.md#overlay-transparency-oa) are set in a single transformation and [obg](overlay.md#overlay-background-obg) has an alpha component, then that value is used to set overlay background transparency. Otherwise, [oa](overlay.md#overlay-transparency-oa) value is used .
{% endhint %}

{% tabs %}
{% tab title="obg=00AAFF" %}
URL - [https://ik.imagekit.io/demo/tr:obg-00FFFF,oh-50,ow-600,ofo-bottom/medium\_cafe\_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:obg-00FFFF,oh-50,ow-600,ofo-bottom/medium_cafe_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:obg-00FFFF,oh-50,ow-600,ofo-bottom/medium_cafe_B1iTdD0C.jpg)
{% endtab %}

{% tab title="obg=00AAFF55 \(55% opacity\)" %}
URL - [https://ik.imagekit.io/demo/tr:obg-00FFFF55,oh-50,ow-600,ofo-bottom/medium\_cafe\_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:obg-00FFFF55,oh-50,ow-600,ofo-bottom/medium_cafe_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:obg-00FFFF55,oh-50,ow-600,ofo-bottom/medium_cafe_B1iTdD0C.jpg)
{% endtab %}
{% endtabs %}

### Overlay focus - \(ofo\)

You can control the relative position of overlay image using `ofo` parameter. This position is relative to base image.

Possible values include `center` , `top` , `left` , `bottom` , `right` , `top_left` , `top_right` , `bottom_left` , and `bottom_right` .  
  
**Default Value**- `center`

{% tabs %}
{% tab title="Default center" %}
URL - [https://ik.imagekit.io/demo/tr:oi-logo-white\_SJwqB4Nfe.png/medium\_cafe\_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:oi-logo-white_SJwqB4Nfe.png/medium_cafe_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:oi-logo-white_SJwqB4Nfe.png/medium_cafe_B1iTdD0C.jpg)
{% endtab %}

{% tab title="Top" %}
URL - [https://ik.imagekit.io/demo/tr:oi-logo-white\_SJwqB4Nfe.png,ofo-top/medium\_cafe\_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:oi-logo-white_SJwqB4Nfe.png,ofo-top/medium_cafe_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:oi-logo-white_SJwqB4Nfe.png,ofo-top/medium_cafe_B1iTdD0C.jpg)
{% endtab %}

{% tab title="Bottom left" %}
URL - [https://ik.imagekit.io/demo/tr:oi-logo-white\_SJwqB4Nfe.png,ofo-bottom\_left/medium\_cafe\_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:oi-logo-white_SJwqB4Nfe.png,ofo-bottom_left/medium_cafe_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:oi-logo-white_SJwqB4Nfe.png,ofo-bottom_left/medium_cafe_B1iTdD0C.jpg)
{% endtab %}
{% endtabs %}

### Overlay X position - \(ox\)

Used to provide granular control over the position of the overlay image relative to the base image.

The top left corner of the input image is considered as \(0,0\), and the bottom right corner as \(w,h\), where w is the width and h is the height of the input image. If `ox`  is 100, then the overlay image is placed at 100px from the left edge of the input image. If the `ox` value exceeds the dimensions of the input image, the values are adjusted and made equal to the corresponding dimension of the input image. Negative values are also supported with a leading N in front of the provided value. 

**Possible Values** include any positive integer. If prefixed with `N` , then treated as a negative integer.

{% tabs %}
{% tab title="X=35" %}
URL - [https://ik.imagekit.io/demo/tr:oi-logo-white\_SJwqB4Nfe.png,ox-35/medium\_cafe\_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:oi-logo-white_SJwqB4Nfe.png,ox-35/medium_cafe_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:oi-logo-white_SJwqB4Nfe.png,ox-35/medium_cafe_B1iTdD0C.jpg)
{% endtab %}

{% tab title="X=N35 \(Negative value\)" %}
URL - [https://ik.imagekit.io/demo/tr:oi-logo-white\_SJwqB4Nfe.png,ox-N35/medium\_cafe\_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:oi-logo-white_SJwqB4Nfe.png,ox-N35/medium_cafe_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:oi-logo-white_SJwqB4Nfe.png,ox-N35/medium_cafe_B1iTdD0C.jpg)
{% endtab %}
{% endtabs %}

### Overlay Y position - \(oy\)

Used to provide granular control over the position of the overlay image relative to the base image. 

The top left corner of the input image is considered as \(0,0\), and the bottom right corner as \(w, h\), where w is the width and h is the height of the input image.  If `oy`  is 150, then the overlay image is placed at 150px from the top edge of the input image. If the `oy` value exceeds the dimensions of the input image, the values are adjusted and made equal to the corresponding dimension of the input image. Negative values are also supported with a leading N in front of the provided value.

**Possible Values** include any positive integer. If prefixed with `N` , then treated as a negative integer.

{% tabs %}
{% tab title="Y=30" %}
URL - [https://ik.imagekit.io/demo/tr:oi-logo-white\_SJwqB4Nfe.png,oy-30/medium\_cafe\_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:oi-logo-white_SJwqB4Nfe.png,oy-30/medium_cafe_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:oi-logo-white_SJwqB4Nfe.png,oy-30/medium_cafe_B1iTdD0C.jpg)
{% endtab %}

{% tab title="Y=N30 \(Negative value\)" %}
URL - [https://ik.imagekit.io/demo/tr:oi-logo-white\_SJwqB4Nfe.png,oy-N30/medium\_cafe\_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:oi-logo-white_SJwqB4Nfe.png,oy-N30/medium_cafe_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:oi-logo-white_SJwqB4Nfe.png,oy-N30/medium_cafe_B1iTdD0C.jpg)
{% endtab %}

{% tab title="X=35 and Y=30" %}
URL - [https://ik.imagekit.io/demo/tr:oi-logo-white\_SJwqB4Nfe.png,ox-35,oy-30/medium\_cafe\_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:oi-logo-white_SJwqB4Nfe.png,ox-35,oy-30/medium_cafe_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:oi-logo-white_SJwqB4Nfe.png,ox-35,oy-30/medium_cafe_B1iTdD0C.jpg)
{% endtab %}
{% endtabs %}

### Overlay height - \(oh\)

Used to specify the height of the overlay image.

{% hint style="info" %}
Using both [oh](overlay.md#overlay-height-oh) and [ow](overlay.md#overlay-width-ow) parameters together will crop the overlay image.
{% endhint %}

{% tabs %}
{% tab title="oh=20" %}
URL - [https://ik.imagekit.io/demo/tr:oi-logo-white\_SJwqB4Nfe.png,oh-20/medium\_cafe\_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:oi-logo-white_SJwqB4Nfe.png,oh-20/medium_cafe_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:oi-logo-white_SJwqB4Nfe.png,oh-20/medium_cafe_B1iTdD0C.jpg)
{% endtab %}

{% tab title="oh=30" %}
URL - [https://ik.imagekit.io/demo/tr:oi-logo-white\_SJwqB4Nfe.png,oh-30/medium\_cafe\_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:oi-logo-white_SJwqB4Nfe.png,oh-30/medium_cafe_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:oi-logo-white_SJwqB4Nfe.png,oh-30/medium_cafe_B1iTdD0C.jpg)
{% endtab %}
{% endtabs %}

### Overlay width - \(ow\)

Used to specify the width of the overlaid image.

{% hint style="info" %}
Using both [oh](overlay.md#overlay-height-oh) and [ow](overlay.md#overlay-width-ow) parameters together will crop the overlay image.
{% endhint %}

{% tabs %}
{% tab title="ow=50" %}
URL - [https://ik.imagekit.io/demo/tr:oi-logo-white\_SJwqB4Nfe.png,ow-50/medium\_cafe\_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:oi-logo-white_SJwqB4Nfe.png,ow-50/medium_cafe_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:oi-logo-white_SJwqB4Nfe.png,ow-50/medium_cafe_B1iTdD0C.jpg)
{% endtab %}

{% tab title="ow=100" %}
URL - [https://ik.imagekit.io/demo/tr:oi-logo-white\_SJwqB4Nfe.png,ow-100/medium\_cafe\_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:oi-logo-white_SJwqB4Nfe.png,ow-100/medium_cafe_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:oi-logo-white_SJwqB4Nfe.png,ow-100/medium_cafe_B1iTdD0C.jpg)
{% endtab %}
{% endtabs %}

### Overlay trimming - \(oit\)

Used to trim overlay images. This removes the transparency of the overlaid image.  
  
**Default Value**- `true`

Learn more from [example here](overlay.md#trimming-of-the-overlay-image).

## Text overlay

### Overlay text - \(ot\)

To overlay text on an image. Supported characters include all alphabets, numbers, spaces, \_, -, %, !, @, and &.

{% tabs %}
{% tab title="Base image" %}
URL - [https://ik.imagekit.io/demo/medium\_cafe\_B1iTdD0C.jpg](https://ik.imagekit.io/demo/medium_cafe_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/medium_cafe_B1iTdD0C.jpg)
{% endtab %}

{% tab title="Text overlay example" %}
URL - [https://ik.imagekit.io/demo/tr:ot-overlay%20made%20easy,ots-45/medium\_cafe\_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:ot-overlay%20made%20easy,ots-45/medium_cafe_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:ot-overlay%20made%20easy,ots-45/medium_cafe_B1iTdD0C.jpg)

Font size is set to 45 using [ots](overlay.md#overlay-text-size-ots) parameter.
{% endtab %}
{% endtabs %}

### Overlay text encoded - \(ote\)

It is similar to [overlay text \(ot\)](overlay.md#overlay-text-ot) but it allows you to overlay special unicode characters \(e.g. ©, ®, ™ etc.\) that you cannot otherwise pass in a URL as plain string. It accepts base64 string.

{% hint style="info" %}
1. The base64 string should be made URL safe to ensure that all padding characters\(=\) are included correctly. A function like `encodeUri()` can be used for this.
2. Input above the length of 144 characters will be truncated.
{% endhint %}

Example ****URL **-** [https://ik.imagekit.io/demo/tr:ote-b3ZlcmxheSBtYWRlIGVhc3k%3D,ots-45/medium\_cafe\_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:ote-b3ZlcmxheSBtYWRlIGVhc3k%3D,ots-45/medium_cafe_B1iTdD0C.jpg)

Here `b3ZlcmxheSBtYWRlIGVhc3k%3D` is the base64 string for "overlay made easy"

![](https://ik.imagekit.io/demo/tr:ote-b3ZlcmxheSBtYWRlIGVhc3k%3D,ots-45/medium_cafe_B1iTdD0C.jpg)

### Overlay text width - \(otw\)

To specify the maximum width \(in pixels\) of the overlaid text on the image. The text is wrapped so that any words in a line that go beyond the given width are sent to the next line. Height of the text box is calculated automatically based on the total number of lines.

**Possible values** include any integer.

Example URL - [https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,ots-45,otc-00FFFF,otw-200/medium\_cafe\_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,ots-45,otc-00FFFF,otw-200/medium_cafe_B1iTdD0C.jpg)

{% tabs %}
{% tab title="otw=200" %}
URL -  [https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,ots-45,otc-00FFFF,otw-200/medium\_cafe\_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,ots-45,otc-00FFFF,otw-200/medium_cafe_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,ots-45,otc-00FFFF,otw-200/medium_cafe_B1iTdD0C.jpg)

Overlay text color is controlled using [otc](overlay.md#overlay-text-color-otc) parameter.
{% endtab %}

{% tab title="otw=400" %}
URL - [https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,ots-45,otc-00FFFF,otw-400/medium\_cafe\_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,ots-45,otc-00FFFF,otw-400/medium_cafe_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,ots-45,otc-00FFFF,otw-400/medium_cafe_B1iTdD0C.jpg)
{% endtab %}
{% endtabs %}

### Overlay text background - \(otbg\)

If you want to give the text block a solid background color \(and transparency\), use this parameter.

**Possible Values** - Valid RGB Hex Code with optional alpha component

Example values - `00AAFF` \(solid color\) or `00AAFF55` \(with 55% opacity\)

#### Overlay text background transparency

This is supported via alpha component in RGB hex code which takes a numeric value between `00` and `99`. 

{% tabs %}
{% tab title="With solid color background" %}
URL - [https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,ots-45,otc-000000,otbg-FFFFFF/medium\_cafe\_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,ots-45,otc-000000,otbg-FFFFFF/medium_cafe_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,ots-45,otc-000000,otbg-FFFFFF/medium_cafe_B1iTdD0C.jpg)
{% endtab %}

{% tab title="Solid color background with transparency" %}
URL - [https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,ots-45,otc-000000,otbg-FFFFFF75/medium\_cafe\_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,ots-45,otc-000000,otbg-FFFFFF75/medium_cafe_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,ots-45,otc-000000,otbg-FFFFFF75/medium_cafe_B1iTdD0C.jpg)
{% endtab %}
{% endtabs %}

### Overlay text padding - \(otp\)

To specify the padding around the overlaid text on the image.

**Possible values** include any positive integer.

{% tabs %}
{% tab title="otp=40" %}
URL - [https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,ots-45,otc-000000,otbg-FFFFFF80,otp-40/medium\_cafe\_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,ots-45,otc-000000,otbg-FFFFFF80,otp-40/medium_cafe_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,ots-45,otc-000000,otbg-FFFFFF80,otp-40/medium_cafe_B1iTdD0C.jpg)
{% endtab %}

{% tab title="otp=40 and otw=300" %}
URL - [https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,ots-45,otc-000000,otw-300,otbg-FFFFFF80,otp-40/medium\_cafe\_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,ots-45,otc-000000,otw-300,otbg-FFFFFF80,otp-40/medium_cafe_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,ots-45,otc-000000,otw-300,otbg-FFFFFF80,otp-40/medium_cafe_B1iTdD0C.jpg)
{% endtab %}
{% endtabs %}

### Overlay text inner alignment - \(otia\)

To specify the alignment of the overlaid text on the image when text is wrapped with overlay text width \(otw\).

**Possible Values** include `left`, `right` and `center` \(default\). 

{% tabs %}
{% tab title="Left align" %}
URL - [https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,ots-45,otc-000000,otw-300,otbg-FFFFFF80,otp-40,otia-left/medium\_cafe\_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,ots-45,otc-000000,otw-300,otbg-FFFFFF80,otp-40,otia-left/medium_cafe_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,ots-45,otc-000000,otw-300,otbg-FFFFFF80,otp-40,otia-left/medium_cafe_B1iTdD0C.jpg)
{% endtab %}

{% tab title="Right align" %}
URL - [https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,ots-45,otc-000000,otw-300,otbg-FFFFFF80,otp-40,otia-right/medium\_cafe\_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,ots-45,otc-000000,otw-300,otbg-FFFFFF80,otp-40,otia-right/medium_cafe_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,ots-45,otc-000000,otw-300,otbg-FFFFFF80,otp-40,otia-right/medium_cafe_B1iTdD0C.jpg)
{% endtab %}
{% endtabs %}

### Overlay text color - \(otc\)

To specify the color and transparency of the overlaid text on the image.   
  
**Possible Values:** Valid RGB Hex Code with optional alpha component

For example - `00AAFF` \(solid color\) or `00AAFF55` \(with 55% opacity\)

**Overlay Text Transparency** is supported via alpha component in RGB hex code which takes a numeric value between 00 and 99.

{% hint style="info" %}
If both [otc](overlay.md#overlay-text-color-otc) and [oa](overlay.md#overlay-transparency-oa) are set in a single transformation and [otc](overlay.md#overlay-text-color-otc) has an alpha component, then this value is used to set overlay text transparency. Otherwise, [oa](overlay.md#overlay-transparency-oa) value is used.
{% endhint %}

{% tabs %}
{% tab title="otc=00AAFF \(no transparancy\)" %}
URL - [https://ik.imagekit.io/demo/tr:ot-overlay%20made%20easy,ots-45,otc-00FFFF/medium\_cafe\_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:ot-overlay%20made%20easy,ots-45,otc-00FFFF/medium_cafe_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:ot-overlay%20made%20easy,ots-45,otc-00FFFF/medium_cafe_B1iTdD0C.jpg)
{% endtab %}

{% tab title="otc=00FFFF55 \(55% opacity\)" %}
URL - [https://ik.imagekit.io/demo/tr:ot-overlay%20made%20easy,ots-45,otc-00FFFF55/medium\_cafe\_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:ot-overlay%20made%20easy,ots-45,otc-00FFFF55/medium_cafe_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:ot-overlay%20made%20easy,ots-45,otc-00FFFF55/medium_cafe_B1iTdD0C.jpg)
{% endtab %}
{% endtabs %}

### Overlay text font - \(otf\)

To specify the font of the overlaid text on the image. You can choose [any font from this list](supported-text-font-list.md#in-built-fonts) or use a [custom font](overlay.md#using-custom-fonts). 

{% tabs %}
{% tab title="otf = Open Sans" %}
URL - [https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,ots-45,otc-00FFFF,otf-Open%20Sans/medium\_cafe\_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,ots-45,otc-00FFFF,otf-Open%20Sans/medium_cafe_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,ots-45,otc-00FFFF,otf-Open%20Sans/medium_cafe_B1iTdD0C.jpg)
{% endtab %}

{% tab title="otf = Roboto" %}
URL - [https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,ots-45,otc-00FFFF,otf-Roboto/medium\_cafe\_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,ots-45,otc-00FFFF,otf-Roboto/medium_cafe_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,ots-45,otc-00FFFF,otf-Roboto/medium_cafe_B1iTdD0C.jpg)
{% endtab %}
{% endtabs %}

#### Using custom fonts

You can use a custom font in text overlay. Upload the font file in your media library and pass the path in `otf` parameter.

{% tabs %}
{% tab title="Text overlay using custom font" %}
URL - [https://ik.imagekit.io/demo/tr:otf-Lacquer-Regular\_nGqtVsBJT.ttf,ot-Hello%20World,ots-72,otc-FFF000/default-image.jpg](https://ik.imagekit.io/demo/tr:otf-Lacquer-Regular_nGqtVsBJT.ttf,ot-Hello%20World,ots-72,otc-FFF000/default-image.jpg)

![](https://ik.imagekit.io/demo/tr:otf-Lacquer-Regular_nGqtVsBJT.ttf,ot-Hello%20World,ots-72,otc-FFF000/default-image.jpg)

**Note:**  
Here the font-file is available at path `/Lacquer-Regular_nGqtVsBJT.ttf` in media library. You will need to pass the whole path including extension.
{% endtab %}
{% endtabs %}

### Overlay text size - \(ots\)

To specify the size of the overlaid text on the image.   
  
**Possible Values** include any integer.

{% tabs %}
{% tab title="ots=45" %}
URL - [https://ik.imagekit.io/demo/tr:ot-overlays made easy,ots-45,otc-00FFFF/medium\_cafe\_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,ots-45,otc-00FFFF/medium_cafe_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,ots-45,otc-00FFFF/medium_cafe_B1iTdD0C.jpg)
{% endtab %}

{% tab title="ots=60" %}
URL - [https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,ots-60,otc-00FFFF/medium\_cafe\_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,ots-60,otc-00FFFF/medium_cafe_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,ots-60,otc-00FFFF/medium_cafe_B1iTdD0C.jpg)
{% endtab %}
{% endtabs %}

### Overlay text typography - \(ott\)

To specify the typography of the font used for the overlaid image. 

**Possible Values** include bold `b`  and italics `i`.

{% tabs %}
{% tab title="Bold" %}
URL - [https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,ots-45,ott-b,otc-00FFFF/medium\_cafe\_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,ots-45,ott-b,otc-00FFFF/medium_cafe_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,ots-45,ott-b,otc-00FFFF/medium_cafe_B1iTdD0C.jpg)
{% endtab %}

{% tab title="Italics" %}
URL - [https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,ots-45,ott-i,otc-00FFFF/medium\_cafe\_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,ots-45,ott-i,otc-00FFFF/medium_cafe_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,ots-45,ott-i,otc-00FFFF/medium_cafe_B1iTdD0C.jpg)
{% endtab %}
{% endtabs %}

### Overlay transparency - \(oa\)

To specify the transparency level of the overlaid image.   
  
**Possible Values** include integers from `1`  to `9` .  
  
**Note:** Overlay Transparency is currently supported for overlay texts. It applies to overlay backgrounds only if alpha component is not set in [obg](overlay.md#overlay-background-obg) parameter.

{% tabs %}
{% tab title="oa=5" %}
URL - [https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,oa-5,ots-45/medium\_cafe\_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,oa-5,ots-45/medium_cafe_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,oa-5,ots-45/medium_cafe_B1iTdD0C.jpg)
{% endtab %}

{% tab title="oa=2" %}
URL - [https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,oa-2,ots-45/medium\_cafe\_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,oa-2,ots-45/medium_cafe_B1iTdD0C.jpg)

![](https://ik.imagekit.io/demo/tr:ot-overlays%20made%20easy,oa-2,ots-45/medium_cafe_B1iTdD0C.jpg)
{% endtab %}
{% endtabs %}

