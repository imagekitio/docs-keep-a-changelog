# Default Images

Default Images are used as placeholders when the image requested using ImageKit.io does not exist. Instead of delivering a broken link or returning a 404 error, a default image can be delivered. ImageKit.io provides an easy way to achieve this without making changes at the application level.

The parameter `di` specifies the path of the image that must be delivered when the original image is not available. The default image must be available within the [Media library](../media-library/overview/) and not in your origin. All the [transformations ](image-transformations/)applied to the original image is applied to the default image as well, before delivering it.

For example: If the default image is:

Default Image: `https://ik.imagekit.io/demo/medium_cafe_B1iTdD0C.jpg`

The following URL fetches the default image when the original image isn't available.

`https://ik.imagekit.io/demo/tr:di-medium_cafe_B1iTdD0C.jpg/non_existent_image.jpg`

