# Private images

Private images are images that are not accessible to any end-user. Since much effort goes into producing an original image, there might be scenarios where original images must be protected and made inaccessible to anyone. For example:

* A paid image gallery that shares original images with its customers only when the payment is completed. 
* You watermark all your images using ImageKit.io parameters. You do not want anyone to access the image URL from your website, remove the transformation parameter to obtain the original image. Or change the transformation string to remove the watermark altogether. 

## Private Images with ImageKit.io

Any original image that is marked private cannot be accessed directly through a standard image URL. Private Original Images can only be accessed through a [valid signed URL.](signed-urls.md)

The same logic applies when a private image is being resized using real-time [transformation parameters](signed-urls.md). A [valid signed URL](/imagekit-docs/signed-urls) is required to access the transformation of a private image.

However, if there are [named transformations](../named-transformations.md) specified within the dashboard, these named transformations can be used to transform your images. Named transformations can be used with private images without generating a valid signed URL.

## Marking Your Images Private

1. If you are [uploading images](../../media-library/overview/upload-files.md) to the [Media library](../../media-library/overview/), pass the parameter `isPrivateFile` as `true` in the image upload request. The uploaded image within the Media Library would be marked as private. 
2. If you are delivering images from your [S3 bucket](../../integration/configure-origin/amazon-s3-bucket-origin.md) added as an [origin](../../integration/configure-origin/), set the object metadata `x-amz-meta-isprivatefile` as `true` to mark the image as private. This [documentation from AWS](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/add-object-metadata.html) provides an overview of adding metadata to an object within S3.
3. If you are delivering images from an [HTTP Server](../../integration/configure-origin/web-server-origin.md) added as an [origin](../../integration/configure-origin/), add `Is-Private-File` as `true` within the server response to mark the image as private.

{% hint style="warning" %}
If an image is marked as private once, its status cannot be changed. To unmark an image as private, re-upload the file with the same name as the image that is marked private.
{% endhint %}

## Upload Private Images using Upload API

You can mark images as private while uploading using the [Upload API](../../api-reference/upload-file-api/).

