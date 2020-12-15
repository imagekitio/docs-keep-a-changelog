---
description: >-
  ImageKit, with its support of client hints, can automatically deliver
  responsive images without you changing the image URL. Learn the different ways
  you can use client hints.
---

# Client hints

## What are Client Hints?

Client hints are the hints provided by the client device to the server along with the request itself. These hints allow the server to fulfill a particular request with the most optimal resource

You can [learn more about clients](https://imagekit.io/responsive-images/#chapter-7---using-client-hints) hints from the responsive image guide.

{% hint style="info" %}
**Enable client hints before using them**  
Not every request has these HTTP headers. You will have to explicitly tell the browser to include these client hints using a meta tag:

```markup
<meta http-equiv="Accept-CH" content="DPR, Width">
```
{% endhint %}

## ImageKit supports Client hints

ImageKit supports the following client hints:

* [Width](https://imagekit.io/responsive-images/#width)
* [DPR](https://imagekit.io/responsive-images/#dpr)
* [Save-Data](https://imagekit.io/responsive-images/#save-data)

{% hint style="info" %}
WebP conversion using `Accept` header is enabled by default and part of [automatic format conversion](image-optimization/automatic-image-format-conversion.md) feature.
{% endhint %}

To allow ImageKit to read values from the client hint request headers \(DPR and Width\), you have to pass the [transformation parameters](https://docs.imagekit.io/features/image-transformations) `dpr` and `width` with their values set to `auto`.  
  
__For example, when the browser requests:

```bash
GET: https://ik.imagekit.io/your_imagekitid/tr:w-100,dpr-auto/image_name.jpg
DPR: 2
```

In this case an [extrinsic width](https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/client-hints) of 100 pixels is required. To calculate the extrinsic width of the to-be-delivered image, ImageKit reads the client hint header `DPR` value and multiplies it with the specified extrinsic width. Therefore, the final actual width of the delivered image is `200`

$$
100 * 2 = 200 px
$$

If the browser requests:

```text
https://ik.imagekit.io/your_imagekitid/tr:w-auto,dpr-auto/image_name.jpg
DPR: 2
Width: 600
```

In this case an intrinsic width of 600 pixels is required.  The browser sends `Width` considering the DPR value. Therefore, ImageKit is ignores the `DPR` valueand delivers an image of width `600`. In this case ImageKit will also return `Content-DPR` respomse header so that browser can scale the image correctly. 

### The Content-DPR Header

ImageKit rounds the intrinsic size of the image to the next smallest 100. If the `Width`header indicates a width of 150 px, then ImageKit will deliver an image with a width of 200 px.  
Now, if the DPR of the device is 2,  then the device will end up rendering an image of width 100 px \(200 / 2\), which is the incorrect width. The correct intended width to be displayed is 75px \(150 / 2\). To rectify this miscalculation due to the rounding to the next 100, the Content-DPR header is used.

Content-DPR is a response header and indicates the actual DPR of the response image. It is calculated as follows:

$$
Content-DPR = [Selected Image Size] / (Width / DPR)
$$

Let's learn this with a few examples:

```text
GET: https://ik.imagekit.io/your_imagekitid/tr:w-auto,dpr-auto/image_name.jpg
DPR: 2
Width: 212
```

ImageKit rounds off `212` to 300, and an image of width 300 pixels is delivered. Now, the Content-DPR header is calculated as follows:

$$
Content-DPR = 300/ (212 / 2) = 2.83
$$

Hence, ImageKit responds with a `300` pixels wide image and `Content-DPR` response header with value `2.83`

```text
Content-DPR: 2.83
```

When the browser receives the image and the header, it scales it down as 300 / 2.83 = 106 px, which was is intended final width of the rendered image. If there were no `Content-DPR` header received, the browser would scale down the image as 300 / 2 = 150 px, which might break your layout.

