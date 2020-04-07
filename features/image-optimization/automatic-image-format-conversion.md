# Automatic image format conversion

With multiple image formats to choose from, it often becomes a difficult task to ensure that the images on a website are delivered in the right format. Amongst the many optimizations ImageKit.io provides, this is the most important one.

## What is Format Optimization?

Format Optimization is the process of delivering the best image format to the end user, while taking into account various factors such as requesting device capabilities, browser support for certain image formats, image content, [image quality](quality-optimization.md), and more. Ensuring the right format helps you reduce the image size and subsequently the load time.

For Example, the below image gets delivered as WebP for Chrome browser and as JPEG for Safari.

Image URL: [https://ik.imagekit.io/demo/tr:w-200/medium\_cafe\_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:w-200/medium_cafe_B1iTdD0C.jpg)

This behaviour works automatically when the 'Use Best Format For Image Delivery' setting is turned on within the dashboard. This setting is accessible under the Optimization section of [Image Settings](https://imagekit.io/dashboard?redirectTo=settings#settings).

![](../../.gitbook/assets/best-image-format-settings.png)

## Factors Ensuring Right Format Delivery

1. Based on Browser Support: ImageKit.io automatically selects the best format to deliver an image. To perform this, browser support for WebP is checked on every incoming request, and whether WebP is the best format for the image. If the browser supports WebP and images can be delivered as WebP, the image is converted and delivered. If the browser doesn't support WebP \(like Safari\), the next best format is chosen and then delivered.
2. Based on Image Content: Certain file formats like JPEG can be easily delivered as WebP \(on Chrome\) and JPEG \(on Safari\). Additionally, image file formats like PNG, which have transparency, are evaluated based on end-user device support, and the best format \(PNG or WebP\) is delivered. If a PNG image does not contain any transparency, then it becomes a candidate even for the JPEG format.

## URL Based Format Control

If you have disabled 'Use Best Format For Image Delivery' setting, using the `f-auto` parameter in the URL enables automatic format conversion based on the above factors.

Using `f-auto` : [https://ik.imagekit.io/demo/tr:w-200,f-auto/medium\_cafe\_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:w-200,f-auto/medium_cafe_B1iTdD0C.jpg)

The above image is served as WebP on Chrome and JPEG on Safari browser.

Just like `f-auto` automatically selects the best format, you can also force an image format using parameters `f-jpg` , `f-png ,`and more.

`f-jpg` delivers a JPG Image: [https://ik.imagekit.io/demo/img/tr:w-300,f-jpg/default-image.jpg](https://ik.imagekit.io/demo/img/tr:w-300,f-jpg/default-image.jpg)

`f-png` delivers a PNG Image: [https://ik.imagekit.io/demo/img/tr:w-300,f-png/default-image.jpg](https://ik.imagekit.io/demo/img/tr:w-300,f-png/default-image.jpg)

Other formats supported for URL-based control \(other than `f-auto` \) are JPG, PNG, WebP, and GIF.

Conversions to GIF using `f-gif` works only when the input image is GIF as well.

{% hint style="info" %}
By default, ImageKit.io delivers the best format for an image. Use the `orig` [URL parameter]() if you want to deliver the image in original image format.
{% endhint %}

