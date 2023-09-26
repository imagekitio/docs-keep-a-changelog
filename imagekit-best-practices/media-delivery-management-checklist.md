# Media Delivery and Management Essentials Checklist

{% hint style="info" %}
When implementing ImageKit, ensure that you at least cover the items listed under "Must-haves". It would be great to cover those listed under "Good-to-haves" as well.
{% endhint %}

## Media Delivery Must-haves

### 1. Format Optimization
Ensure that you are delivering the images in the correct format. If available, for your account, do get automatic AVIF conversion enabled as well.

For the web, this would be as simple as turning on the automatic format conversion settings from your ImageKit dashboard for both [images](https://docs.imagekit.io/features/image-optimization/automatic-image-format-conversion) and [videos](https://docs.imagekit.io/features/video-optimization/automatic-video-format-conversion).

For mobile apps, in the absence of the right content negotiation headers, it is best to force the output format using URL transformation parameters. The best practices for format optimization for mobile apps have been explained [here](https://docs.imagekit.io/best-practices/mobile-apps#1.-using-format-parameters-to-force-the-format-of-images).


### 2. Compressing the output media
It is possible to further reduce the size of the images and videos via compression without reducing the perceived quality of the output file.

One compression method is removing all the metadata associated with the file that’s not needed for display on the app. You can enable this for image files by setting the Preserve metadata option to “Discard all metadata”, or set it explicitly in the URL transformation parameters. Read more about [image metadata optimization here](https://docs.imagekit.io/features/image-optimization/metadata-color-profile-and-orientation#image-metadata).

The other method uses lossy compression to reduce the file size. However, the human eye cannot perceive the changes in an image or video delivered at a slightly reduced quality. You can turn this on for images by [enabling automatic quality optimization here](https://imagekit.io/dashboard/settings/images). The ideal default quality for images is 80. For videos, you can [enable the same here](https://imagekit.io/dashboard/settings/videos) and keep the default quality at 50.

You can also use the quality transformation parameter directly in the URL, as [documented here](https://docs.imagekit.io/features/image-optimization/quality-optimization#image-quality-using-the-url-parameter).


### 3. Correctly resized media on web and mobile
This is the most important part of optimizing media experiences on the web and apps. Different pages and placeholders require images and videos to be rendered in different sizes. Loading a 2000x2000px image is incorrect when the placeholder is just 250x250px wide. However, you also need to consider high-density displays (like retina displays) that require a higher-resolution image to be loaded to maintain the same visual clarity across devices.

Using ImageKit’s [URL-based transformation parameters](https://docs.imagekit.io/features/image-transformations/resize-crop-and-other-transformations) for height, width, aspect ratio, DPR, and other crop modes will help you attain the right fit you need for your media files.

**Images**

[Implementing responsive images](https://imagekit.io/responsive-images/) for the web is recommended, especially if you have a server-side rendered application. You can read more about responsive images here.

If you have a client-side rendered application, you can get the right size and DPR using the [URL transformation parameters](https://docs.imagekit.io/features/image-transformations/resize-crop-and-other-transformations) when constructing the image URL.

**Videos**

Use the [width, height, and aspect ratio parameters](https://docs.imagekit.io/features/video-transformation/resize-crop-and-other-common-video-transformations) and combine them with the available crop modes to get the video to fit your desired layout.

{% hint style="info" %}
If resizing using width and height parameters, try bucketing the sizes if implementing this technique. For example, for all devices with less than 360px width, render the image at 360px width. For all devices between 360px and 480px width, render the image at 480px width. Keep the number of size variations for a single resource to 5. This reduces the number of generated variants, improving the cache-hit ratio and load performance.
{% endhint %}

### 4. Lazy Loading
You need not load upfront any media not visible to the user on the screen when they first open your website or app. This helps reduce the data to be downloaded and rendered on the user's screen, thereby using the user's device's capabilities and available bandwidth to load and render more critical resources and speeding up the perceived loading experience.

You can do this using several standard libraries on different platforms for images and videos. You can [read more about lazy loading images here](https://imagekit.io/blog/lazy-loading-images-complete-guide/).

{% hint style="info" %}
Use ImageKit to create placeholder content till your actual image loads. For example, you can load a blurred, low-quality version of your image in place of the original image.
{% endhint %}

### 5. PageSpeed or Lighthouse Report for web
Run critical pages of your website, like the home page, product detail page, product listing page, article page, etc., through [Lighthouse](https://developer.chrome.com/docs/lighthouse/overview/) or [PageSpeed](https://pagespeed.web.dev/), or other similar tools. Ensure that no image-related issues are reported. If such issues are reported, ensure that the image is being loaded via ImageKit. Also, see that you have implemented points 1 to 4 for them to [optimize the PageSpeed score](https://imagekit.io/blog/improve-google-pagespeed-insights-score-for-images/).

Run these reports monthly or after major UI updates to ensure your page performance is on track.


### 6. Adaptive Bitrate Streaming for videos
If your website or app loads videos that are over 30 seconds in length, it is recommended to use [Adaptive Bitrate Streaming](https://imagekit.io/blog/video-streaming-api/) for them so that you can adjust the video stream quality to the user’s network bandwidth. This is similar to the experience you get on YouTube, where on poorer network conditions, you see a low-quality video that automatically improves as your network speed improves.

With ImageKit, you can implement DASH and HLS protocols for Adaptive Bitrate Streaming using simple URL transformations. Read more about [Adaptive Bitrate Streaming with ImageKit here](https://docs.imagekit.io/features/video-transformation/adaptive-bitrate-streaming).
If you have any questions, reach out to us on [support@imagekit.io](mailto:support@imagekit.io)


## Media Delivery Good-to-haves

### 1. Set maximum image dimension limits
We often miss out on using suitable transformation parameters in our image URLs. In such cases, if the original image is a high-resolution image, it would consume a lot of bandwidth.

To avoid this, set the maximum image size in the ImageKit dashboard [here](https://imagekit.io/dashboard/settings/images). ImageKit will use this as a size limit for your images when the URL does not contain any height or width resizing parameters.

### 2. Named Transformations
[Named Transformations](https://docs.imagekit.io/features/named-transformations) simplify development efforts by giving easy-to-remember aliases to transformations you use on your website. For example, a transform to generate a watermarked image of size 200x200 (`w-200,h-200,oi-logo.png`) can be referred to as `n-watermarked-thumbnail` in your URL.

Named Transforms also provide an optional [additional layer of security](https://docs.imagekit.io/features/named-transformations#secure-images-with-named-transformations) by limiting the allowed transformations on your account to only the named transformations created by you.


## Media Storage and Management Must-haves

### 1. Upload high-resolution images
A high-resolution, high-quality image or video can always be scaled down using ImageKit. But the reverse is not true. Start with a low-resolution file, and you get restricted to using that image on all devices, even those requiring a higher resolution.

For images, it is generally recommended to start above 1500 x 1500px in dimensions. You can scale them down using the resizing parameters in ImageKit. Similarly, start with HD or Full HD for videos and then scale it down.

### 2. No or minimal compression before ImageKit
Refrain from compressing your media files when exporting in Photoshop or Premier Pro. ImageKit can do all the compression for you in one go. Limiting all the compression in a single pass at Imagekit also ensures that the output is at a higher quality compared to multiple optimization levels.

Using “[Save for web](https://helpx.adobe.com/in/photoshop-elements/using/optimizing-images.html)” with a compression level of 95 in Photoshop often works fine for images.

### 3. Organizing files in folders or by tags and custom metadata
Getting the folder hierarchy right is extremely important when dealing with a large volume of media assets across different categories. It helps build a shared knowledge base within the team of where to store and, more importantly, find the right content.

ImageKit’s media library also offers [content tagging](https://docs.imagekit.io/media-library/overview/image-tags) and [custom metadata](https://docs.imagekit.io/api-reference/custom-metadata-fields-api) to build your custom organization scheme. Using these, you can [search for files](https://imagekit.io/blog/how-to-use-media-librarys-advanced-search-for-your-digital-assets/) not just by their names or the folders they are in but also by attributes like their collar, product category, intended use, and more, which are relevant to your business or use case.

Read more about [organizing your digital assets](https://imagekit.io/blog/organizing-digital-assets/) here.

### 4. Storing URLs from upload and file IDs in your DB
When using APIs to upload and manage images in ImageKit’s media library, ensure that you store the File ID you get in response to the [file upload API](https://docs.imagekit.io/api-reference/upload-file-api). The File ID is the unique identifier that can be used across multiple operations like file details, delete, copy, move, etc.

## Media Storage and Management Good-to-haves

### 1. Upload high-resolution images
ImageKit’s media library has a [built-in image editor](https://docs.imagekit.io/media-library/overview/edit-image) and [AI-powered background removal](https://docs.imagekit.io/extensions/overview). So, suppose you quickly want to replace the background of your image or make a simple change before you post your image on social media instead of going to your graphic designers. In that case, you can do it directly in the browser using the built-in editors. This reduces the requirement for additional software licenses and saves time, improving your team’s velocity in rolling out new campaigns.
