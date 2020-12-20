# Automatic image format conversion

With multiple image formats to choose from, it often becomes a difficult task to ensure that the images on a website are delivered in the right format. Amongst the many optimizations ImageKit.io provides, this is the most important one.

## What is Format Optimization?

Format Optimization is the process of delivering the best image format to the end-user, while taking into account various factors such as requesting device capabilities, browser support for certain image formats, image content, [image quality](quality-optimization.md), and more. Ensuring the right format helps you reduce the image size and subsequently the load time.

For Example, the below image gets delivered as WebP for Chrome browser and as JPEG for Safari. If you have the automatic conversion to [AVIF enabled for your account](automatic-image-format-conversion.md#avif-image-format-support), then the image will be delivered in AVIF format on supported devices.

Image URL: [https://ik.imagekit.io/demo/tr:w-200/medium\_cafe\_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:w-200/medium_cafe_B1iTdD0C.jpg)

This behaviour works automatically when the 'Use Best Format For Image Delivery' setting is turned on within the dashboard. This setting is accessible under the Optimization section of [Image Settings](https://imagekit.io/dashboard?redirectTo=settings#settings).

![](../../.gitbook/assets/best-image-format-settings.png)

## Factors Ensuring Right Format Delivery

1. Based on Browser Support: ImageKit.io automatically selects the best format to deliver an image. To perform this, browser support for formats like WebP and AVIF is checked on every incoming request. If the browser supports either of these image formats, and the image can be converted to them, the ImageKit automatically selects the format with the smallest size for delivery. If the browser doesn't support these formats \(like Safari\), the next best format is selected and then delivered.
2. Based on Image Content: Certain file formats like JPEG can be easily [delivered as AVIF](automatic-image-format-conversion.md#avif-image-format-support) \(on Chrome 85+\) or WebP \(on Chrome\). For other image file formats like PNG which have transparency, only those formats that support transparency, like PNG or WebP or AVIF are considered for delivery. If a PNG image does not contain any transparency, then it becomes a candidate even for the JPEG format.

## URL Based Format Control

If you have disabled the 'Use Best Format For Image Delivery' setting, using the `f-auto` parameter in the URL enables automatic format conversion based on the above factors.

Using `f-auto` : [https://ik.imagekit.io/demo/tr:w-200,f-auto/medium\_cafe\_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:w-200,f-auto/medium_cafe_B1iTdD0C.jpg)

The above image is served as WebP on Chrome \(or AVIF on Chrome 85+ if you have [AVIF auto-conversion feature enabled](automatic-image-format-conversion.md#avif-image-format-support)\) and JPEG on Safari browser.

Just like `f-auto` automatically selects the best format, you can also force an image format using parameters `f-jpg` , `f-png ,`and more.

`f-jpg` delivers a JPG Image: [https://ik.imagekit.io/demo/img/tr:w-300,f-jpg/default-image.jpg](https://ik.imagekit.io/demo/img/tr:w-300,f-jpg/default-image.jpg)

`f-png` delivers a PNG Image: [https://ik.imagekit.io/demo/img/tr:w-300,f-png/default-image.jpg](https://ik.imagekit.io/demo/img/tr:w-300,f-png/default-image.jpg)

Other formats supported for URL-based control \(other than `f-auto` \) are JPG, PNG, WebP, AVIF, and GIF.

Conversions to GIF using `f-gif` works only when the input image is GIF as well.

{% hint style="info" %}
By default, ImageKit.io delivers the best format for an image. Use the`orig-true` transformation parameter if you want to get the original image as it is without any optimization or transformation.
{% endhint %}

## AVIF Image Format Support

AVIF \(AV1 Image File\) is a new image format that offers superior compression and visual quality compared to other image formats like JPEG and WebP. It has been launched in Chrome 85, and support is likely to be added in Android and Firefox soon. More details on the current browser support for AVIF can be found here - [https://caniuse.com/avif](https://caniuse.com/avif).

{% hint style="info" %}
Before you start using AVIF images on your website, we would recommend reading this detailed[ blog that highlights the features, compression, visual quality, and the current drawbacks of the AVIF image format](https://imagekit.io/blog/automatic-avif-image-optimization-imagekit/).
{% endhint %}

Starting 15 December 2020, ImageKit has started rolling out support for automatic conversion of images to the AVIF format on devices that support AVIF format; they send `image/avif` in the `Accept` request header. Here is how the rollout is planned -

1. All users can force their images to AVIF format by adding the format transformation parameter `f-avif` in the transformation string
2. Automatic format conversion to AVIF, without making any change to the URL, will be enabled gradually for our current users. You will see if this feature is enabled for your account or not under the Image Settings &gt; Optimization section in your ImageKit dashboard. If this feature is not enabled for your account and you want early access to it, please reach out to support@imagekit.io or initiate a live chat from your dashboard.
3. As of 15 December 2020, users on custom CDNs won't have this access to this feature working. Our team will be reaching out to such users, helping them with the required configuration changes on their CDN, if any, before enabling the feature on their account.
4. All transformations will be supported on AVIF images, except for the Preserve color profile \(through the dashboard setting or the URL parameter `cp-true`\) and Unsharp Mask \(`e-usm`\) transformations. If your transformation string or the dashboard settings use either of these two features, the final output image will not be delivered in AVIF. WebP and other format optimizations will continue to work in such cases.
5. AVIF images as an input image are also supported.

If you have any questions regarding the AVIF format optimization feature for your account or the compression or image quality in AVIF, please reach out to us at support@imagekit.io or via the live chat in your dashboard.

