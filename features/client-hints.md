---
description: >-
  ImageKit, with its support of client hints, can automatically deliver
  responsive images without you changing the image URL. Learn the different ways
  you can use client hints.
---

# Client hints

## What are Client Hints?

Client hints are the hints provided by the client device to the server along with the request itself. These hints allow the server to fulfill a particular request with the most optimal resource.

You can [learn more about client hints](https://imagekit.io/responsive-images/#chapter-7---using-client-hints) from the responsive image guide.

{% hint style="info" %}
**Enable client hints before using them**\
Not every request has these HTTP headers. You will have to explicitly tell the browser to include these client hints using a meta tag:

```markup
<meta http-equiv="Accept-CH" content="Sec-CH-DPR, Sec-CH-Width">
```

## ImageKit supports Client hints

ImageKit supports the following client hints:

* [Sec-CH-Width](https://imagekit.io/responsive-images/#sec-ch-width)
* [Sec-CH-DPR](https://imagekit.io/responsive-images/#sec-ch-dpr)
* [Save-Data](https://imagekit.io/responsive-images/#save-data)

{% hint style="info" %}
WebP conversion using `Accept` header is enabled by default and part of the [automatic format conversion](image-optimization/automatic-image-format-conversion.md) feature.
{% endhint %}

To allow ImageKit to read values from the client hint request headers (DPR and Width), you have to pass the [transformation parameters](https://docs.imagekit.io/features/image-transformations) `dpr` and `width` with their values set to `auto`.

For example, when the browser requests:

```bash
GET: https://ik.imagekit.io/your_imagekitid/tr:w-100,dpr-auto/image_name.jpg
Sec-CH-DPR: 2
```

In this case, an [extrinsic width](client-hints.md#extrinsic-size) of 100 pixels is required. To calculate the extrinsic width of the to-be-delivered image, ImageKit reads the client hint header `Sec-CH-DPR` value and multiplies it with the specified extrinsic width. Therefore, the final actual width of the delivered image is `200`

$$
100 * 2 = 200 px
$$

If the browser requests:

```
https://ik.imagekit.io/your_imagekitid/tr:w-auto,dpr-auto/image_name.jpg
Sec-CH-DPR: 2
Sec-CH-Width: 600
```

In this case, an [intrinsic width](client-hints.md#intrinsic-size) of 600 pixels is required.  The browser sends `Sec-CH-Width` request header and also considers the DPR of the user-device while calculating the value of `Sec-CH-Width` header. Therefore, ImageKit ignores the `Sec-CH-DPR` value and delivers an image of width `600`. ImageKit will return `Content-DPR` response header so that browser can scale the image correctly. 

### The Content-DPR Header

ImageKit rounds the [intrinsic size](client-hints.md#intrinsic-size) of the image to the next smallest 100. If the `Sec-CH-Width`header indicates a width of 150 px, then ImageKit will deliver an image with a width of 200 px.\
Now, if the DPR of the device is 2,  then the device will end up rendering an image of width 100 px (200 / 2), which is the incorrect width. The correct intended width to be displayed is 75px (150 / 2). To rectify this miscalculation due to the rounding to the next 100, the Content-DPR header is used.

Content-DPR is a response header and indicates the actual DPR of the response image. It is calculated as follows:

$$
Content-DPR = [Selected Image Size] / (Width / DPR)
$$

Let's learn this with a few examples:

```
GET: https://ik.imagekit.io/your_imagekitid/tr:w-auto,dpr-auto/image_name.jpg
Sec-CH-DPR: 2
Sec-CH-Width: 212
```

ImageKit rounds off `212` to 300, and an image of width 300 pixels is delivered. Now, the Content-DPR header is calculated as follows:

$$
Content-DPR = 300/ (212 / 2) = 2.83
$$

Hence, ImageKit responds with a `300` pixels wide image and `Content-DPR` response header with a value `2.83`

```
Content-DPR: 2.83
```

When the browser receives the image and the header, it scales it down as 300 / 2.83 = 106 px, which was is intended final width of the rendered image. If there were no `Content-DPR` header received, the browser would scale down the image as 300 / 2 = 150 px, which might break your layout.

## Definitions

You should be aware of the different terms. Here are a few important definitions from [Google developer docs](https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/client-hints).

### Intrinsic size

The actual dimensions of a media resource. For example, if you open an image in Photoshop, the dimensions shown in the image size dialogue describe its intrinsic size.

### **Extrinsic size:**

The size of a media resource after CSS and other layout factors (such as `width` and `height` attributes) have been applied to it. Let’s say you have an `<img>` element that loads an image with a density-corrected intrinsic size of 320x240, but it also has CSS `width` and `height` properties with values of `256px` and `192px` applied to it, respectively. In this example, the _extrinsic size_ of that `<img>` element becomes 256x192.
