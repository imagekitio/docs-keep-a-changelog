# Best practices for integrating ImageKit on mobile apps

{% hint style="info" %}
These practices are applicable while building apps that run natively on Android and iOS. These do not apply to opening websites in a web browser on either of the two platforms. 
{% endhint %}

## 1. Using format parameters to force the format of images
ImageKit uses the standard [content negotiation](https://developer.mozilla.org/en-US/docs/Web/HTTP/Content_negotiation) approach to identify the image formats supported by a device or browser. This content negotiation technique depends on the `Accept` header value in the request. 

Mobile apps often do not send any `Accept` header value. Without this header, ImageKit assumes that the device does not support WebP and continues to deliver images in JPG or PNG. These formats are larger in size than WebP and therefore consume excess bandwidth.

To deliver WebP images, if supported natively on the mobile OS, you should force the output format specifically to WebP using ImageKit's format transformation parameter `f`. 

For example, the following URL has an `f-webp` transformation to force the output to WebP format.

```
https://ik.imagekit.io/demo/default-image.jpg?tr=f-webp
```

{% hint style="info" %}
Note:
1. You can find WebP support for Android here
2. You can find WebP support for iOS (and other Apple OS) here 
3. AVIF support for Android and iOS is still negligible. Use Google to find when it gets supported for apps.
{% endhint %}

## 2. Combining DPR with resizing on mobile apps
Different image placeholders on your app will need images in different sizes. Mobile phones additionally have high-density or retina displays and therefore need to load an image larger in dimensions on such displays. You would have seen this with `@1x`, `@2x` or `xhdpi`, `xxhdpi`, etc., on apps where you provide the same image in different dimensions.

Similarly, when loading images with ImageKit, you must add the appropriate DPR value along with the resize parameters. This can be done using the [`dpr` transformation parameter](../features/image-transformations/resize-crop-and-other-transformations.md#dpr---dpr).

For example, if you need a 300px wide image, you can use the following URLs with varying `dpr` transform values on each display density.

1x - https://ik.imagekit.io/demo/default-image.jpg?tr=w-300,dpr-1
2x - https://ik.imagekit.io/demo/default-image.jpg?tr=w-300,dpr-2 
3x - https://ik.imagekit.io/demo/default-image.jpg?tr=w-300,dpr-3

### Important hints
1. You might be able to get sufficiently high visual quality by capping the DPR value to 2 even on higher display densities. This means that using `dpr-2` on a 3x device might still work. Do give it a try.

2. When loading high-resolution images on high-density displays, you might be able to compress them more and still retain the same visual quality. For example, you can compress the 2x image to quality 60 and still get the same output visual quality. You can do this with the quality transformation parameter `q`.

```
https://ik.imagekit.io/demo/default-image.jpg?tr=w-300,dpr-2,q-60
```

   3. The original image on which the transformation is carried out should be large enough in terms of dimensions to allow for DPR transformations. For example, if the original image is 300px wide, then using a transform `w-300,dpr-2` will give you a 600px wide image. But, it would be pixelated because ImageKit had to enlarge a smaller image to a larger dimension.


## 3. Lazy Loading images
Mobile apps often have layouts with multiple or infinite scrolls. However, because of the limited mobile screen size, the user can only see a small portion of the layout at a time. Outside of that visible portion (the viewport) of the layout, any media you load is not visible to the user. Therefore, it might not be helpful to load that media at all.

You must use a technique called lazy loading, where you can defer loading any images or videos that are well outside the viewport to a later time. This helps you save bandwidth and improve the app user experience.

While this article discusses [lazy loading for the web](https://imagekit.io/blog/lazy-loading-images-complete-guide/), the concepts are equally applicable to mobile apps. Do give it a read. 

## 4. Network-based optimization (optional)
Once you have done all of the above, one more optimization you can implement on your apps is network-based media optimization, i.e., load a lighter image or video if the user is experiencing poor network speeds. 

The concept is simple.
1. You determine the user's network speed
2. You load a lighter file by compressing it more (by lowering the quality transformation parameter)

You can read more about these two steps for [implementing network-based optimization in your apps](../features/network-based-media-optimization.md) in detail here.

Additionally, to improve the developer experience and security of your file URLs, do explore features such as named transformations, signed URLs, and advanced security.

If you have any questions, reach out to us on [support@imagekit.io](mailto:support@imagekit.io)