---
description: URL-based parameters to transform images in real-time.
---

# Image Transformations

## URL Based transformations

ImageKit.io can perform real-time transformations to deliver perfect images to the end-users. These transformations can be performed by using the URL-based transformation [parameters](resize-crop-and-other-transformations.md).&#x20;

{% tabs %}
{% tab title="URL structure" %}
```markup
        URL-endpoint        transformation       image path                                    
┌──────────────────────────┐┌────────────┐┌───────────────────────┐
https://ik.imagekit.io/demo/tr:w-300,h-300/medium_cafe_B1iTdD0C.jpg
```
{% endtab %}
{% endtabs %}

* **Original 600x600 image**\
  ****[https://ik.imagekit.io/demo/medium\_cafe\_B1iTdD0C.jpg](https://ik.imagekit.io/demo/medium\_cafe\_B1iTdD0C.jpg)****
* **Resized 300x300 image**\
  ****[https://ik.imagekit.io/demo/`tr:w-300,h-300`/medium\_cafe\_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:w-300,h-300/medium\_cafe\_B1iTdD0C.jpg)

These transformation parameters `w-300,h-300` can be added in the URL as path params or as query parameters.

* **As path parameter** [https://ik.imagekit.io/demo/`tr:w-200,h-200`/medium\_cafe\_B1iTdD0C.jpg](https://ik.imagekit.io/demo/tr:w-200,h-200/medium\_cafe\_B1iTdD0C.jpg)****
* **As query parameter** [https://ik.imagekit.io/demo/medium\_cafe\_B1iTdD0C.jpg?`tr=w-200,h-200`](https://ik.imagekit.io/demo/medium\_cafe\_B1iTdD0C.jpg?tr=w-200,h-200)

{% hint style="info" %}
Images transformation are subject to [certain limits](../../limits-and-troubleshooting/limits.md#image-limits).
{% endhint %}

## Use cases:

The transformations can be as basic as manipulating the height and width of the image, to complex transformations like watermarking or smart cropping of your images. A few common use-cases are explained below.

### 1. Creating multiple variants from a single master image

Often you need multiple variations of a single image to cater to different devices and sections within your application layout. Now you can create as many variations using ImageKit.io real-time image transformation parameters. Just change the URL to get a new image.

**Learn about all commonly used resizing parameters**

{% content-ref url="resize-crop-and-other-transformations.md" %}
[resize-crop-and-other-transformations.md](resize-crop-and-other-transformations.md)
{% endcontent-ref %}

**Learn how to used named transformations for better code readability**

{% content-ref url="../named-transformations.md" %}
[named-transformations.md](../named-transformations.md)
{% endcontent-ref %}

### 2. Low-quality image placeholder&#x20;

You can load a small size blurred image as a placeholder while the actual image loads in the background. ImageKit.io provides a blur parameter `bl` that can give you a blurred file that is smaller in file size but can be used as a placeholder as it shows the content. This technique is used by many applications, including Medium publication.

![https://ik.imagekit.io/demo/medium\_cafe\_B1iTdD0C.jpg?tr=bl-6](<../../.gitbook/assets/lqip (1).jpg>)

### 3. Deliver Responsive and Art Directed Images

With the ability to resize images by changing URL, it becomes very easy to implement [responsive images](https://imagekit.io/responsive-images/) in your web applications. Learn more about responsive images in our detailed guide:

{% embed url="https://imagekit.io/responsive-images/" %}

### 4. Smart-cropping for generating meaningful thumbnails

ImageKit.io can automatically detect the most important part of the image and preserves it in the output thumbnail. This could be super useful while creating small thumbnails. Learn more from the blog post below.

{% embed url="https://imagekit.io/blog/smart-crop-deliver-perfect-responsive-images/" %}
Practical applications of smart crop
{% endembed %}

### 5. Watermarking or image overlay

You can overlay multiple images or colored rectangles on your original image directly from the URL. If you happen to change the overlay image, it can be done in minutes instead of days.

Here we have put the ImageKit.io logo ([https://ik.imagekit.io/demo/logo-white\_SJwqB4Nfe.png](https://ik.imagekit.io/demo/logo-white\_SJwqB4Nfe.png)) on another image using overlay syntac i.e. `l-image,i-logo-white_SJwqB4Nfe.png,l-end`

**Learn more about image overlay**

{% content-ref url="overlay-using-layers.md" %}
[overlay.md](overlay-using-layers.md)
{% endcontent-ref %}

![https://ik.imagekit.io/demo/tr:h-300l-image,i-logo-whit\_SJwqB4Nfe.png,l-end/medium\_cafe\_B1iTdD0C.jpg](../../.gitbook/assets/overlay-image.jpg)

### 6. Text overlay

You can overlay text on an image and create dynamic banners. You can also control the font, font size, weight, color, and position of the text.

**Learn more about text overlay**

{% content-ref url="overlay-using-layers.md" %}
[overlay.md](overlay-using-layers.md)
{% endcontent-ref %}

![https://ik.imagekit.io/demo/tr:l-text,i-overlay%20made%20easy,fs-45,co-00FFFF,l-end/medium\_cafe\_B1iTdD0C.jpg](../../.gitbook/assets/text-overlay.jpg)
