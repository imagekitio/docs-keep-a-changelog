# Automatic image format conversion

With multiple image formats to choose from, it often becomes a difficult task to ensure that the images on a website are delivered in the right format. Amongst the many optimizations ImageKit.io provides, this is the most important one.

## What is Format Optimization?

Format Optimization is the process of delivering the best image format to the end-user, while taking into account various factors such as requesting device capabilities, browser support for certain image formats, image content, [image quality](quality-optimization.md), and more. Ensuring the right format helps you reduce the image size and subsequently the load time.

For Example, the below image gets delivered as WebP for Chrome browser and as JPEG for Safari (if it doesn't support WebP). If you have the automatic conversion to [AVIF enabled for your account](automatic-image-format-conversion.md#avif-image-format-support), then the image will be delivered in AVIF format on supported devices.

Image URL: [https://ik.imagekit.io/demo/tr:w-200/medium_cafe_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:w-200/medium_cafe_B1iTdD0C.jpg)

This behaviour works automatically when the 'Use Best Format For Image Delivery' setting is turned on within the dashboard. This setting is accessible under the Optimization section of [Image Settings](https://imagekit.io/dashboard?redirectTo=settings#settings).

![](../../.gitbook/assets/best-image-format-settings.png)

## Factors Ensuring Right Format Delivery

1. Based on Browser Support: ImageKit.io automatically selects the best format to deliver an image. To perform this, browser support for formats like WebP and AVIF is checked on every incoming request. If the browser supports either of these image formats, and the image can be converted to them, the ImageKit automatically selects the format with the smallest size for delivery. If the browser doesn't support these formats, the next best format is selected and then delivered.
2. Based on Image Content: Certain file formats like JPEG can be easily [delivered as AVIF](automatic-image-format-conversion.md#avif-image-format-support) (on Chrome 85+, for example) or WebP. For other image file formats like PNG which have transparency, only those formats that support transparency, like PNG or WebP or AVIF are considered for delivery. If a PNG image does not contain any transparency, then it becomes a candidate even for the JPEG format.

## URL Based Format Control

If you have disabled the 'Use Best Format For Image Delivery' setting, using the `f-auto` parameter in the URL enables automatic format conversion based on the above factors.

Using `f-auto` : [https://ik.imagekit.io/demo/tr:w-200,f-auto/medium_cafe_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:w-200,f-auto/medium_cafe_B1iTdD0C.jpg)

The above image is served as WebP on Chrome (or AVIF on Chrome 85+ if you have [AVIF auto-conversion feature enabled](automatic-image-format-conversion.md#avif-image-format-support)) and JPEG on Safari browser.

Just like `f-auto` automatically selects the best format, you can also force an image format using parameters `f-jpg` , `f-png ,`and more.

`f-jpg` delivers a JPG Image: [https://ik.imagekit.io/demo/img/tr:w-300,f-jpg/default-image.jpg](https://ik.imagekit.io/demo/img/tr:w-300,f-jpg/default-image.jpg)

`f-png` delivers a PNG Image: [https://ik.imagekit.io/demo/img/tr:w-300,f-png/default-image.jpg](https://ik.imagekit.io/demo/img/tr:w-300,f-png/default-image.jpg)

Other formats supported for URL-based control (other than `f-auto` ) are JPG, PNG, WebP, AVIF, and GIF.

Conversions to GIF using `f-gif` works only when the input image is GIF as well.

{% hint style="info" %}
By default, ImageKit.io delivers the best format for an image. Use the`orig-true` transformation parameter if you want to get the original image as it is without any optimization or transformation.
{% endhint %}

{% hint style="info" %}
A few WebP images may not render correctly in Safari v14+ on MacOS v11+ and IOS 14 because of a [possible OS-level issue](https://bugs.webkit.org/show_bug.cgi?id=219977).
{% endhint %}

## AVIF Image Format Support

AVIF (AV1 Image File) is a new image format that offers superior compression and visual quality compared to other image formats like JPEG and WebP. It has been launched in Chrome 85, and support is likely to be added in other browsers. More details on the current browser support for AVIF can be found here - [https://caniuse.com/avif](https://caniuse.com/avif).

{% hint style="info" %}
Before you start using AVIF images on your website, we would recommend reading this detailed [blog that highlights the features, compression, visual quality, and current drawbacks of the AVIF image format](https://imagekit.io/blog/automatic-avif-image-optimization-imagekit/).
{% endhint %}

ImageKit supports automatic conversion of images to the AVIF format on devices that support AVIF format; they send `image/avif` in the `Accept` request header. This is, however, disabled by default.

1. All users can force their images to AVIF format by adding the format transformation parameter `f-avif` in the transformation string. Please check [expensive transformation limits](/limits-and-troubleshooting/limits.md#expensive-transformation-limits) before using this in production.
2. Automatic format conversion to AVIF, without changing the URL, is disabled by default. If you want early access to this feature, please reach out to support@imagekit.io or initiate a live chat from your dashboard. Our team will suggest if and how this feature can be enabled on your account based on your current use of ImageKit.
3. Users on custom CDNs won't have this access to this feature. Our team will be reaching out to such users, helping them with the required configuration changes on their CDN, if any, before enabling the feature on their account.
4. All transformations will be supported on AVIF images, except for the Preserve color profile (through the dashboard setting or the URL parameter `cp-true`) and Unsharp Mask (`e-usm`) transformations. If your transformation string or the dashboard settings use either of these two features, the final output image will not be delivered in AVIF. WebP and other format optimizations will continue to work in such cases.
5. AVIF images as input images are also supported.

If you have any questions regarding the AVIF format optimization feature for your account or the compression or image quality in AVIF, please reach out to us at support@imagekit.io or via the live chat in your dashboard.

## HEIF (HEIC) Image Format Support

**High-Efficiency Image File Format** (**HEIF**) is a container format for individual images and image sequences. It allows storing high-quality images at smaller sizes when compared to JPEG format. Apple adopted this format for its iOS devices and made HEIC, a container format, the default format to store photos on the devices.

While HEIF or HEIC files work well on iOS devices, browsers cannot load them. They have to be converted to formats that browsers support like JPEG, WebP, etc.

ImageKit now supports decoding HEIF and HEIC images to web-safe image formats - JPG, PNG, and WebP. ImageKit will automatically determine the formats supported on the requesting device and convert the image to the appropriate format. You can carry out all ImageKit transformations on these images.

Currently, HEIF images are not automatically converted to AVIF format, even if auto-conversion to AVIF is enabled for your account or the browser supports it. However, you can convert them to AVIF using the format transformation parameter `f-avif`.

You can download the original HEIF or HEIC image by adding the `orig-true` transformation parameter. Currently, the encoding of images to HEIF is not supported in ImageKit, i.e., you cannot convert a non-HEIF image to HEIF via ImageKit.

