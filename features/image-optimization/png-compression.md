# PNG Compression

ImageKit.io performs lossy compression for all your images by default. Lossy compression helps deliver smaller-sized files with good perceived visual quality.

However, certain images like computer-generated graphics and logo designs aren't great with lossy compression. In such cases, lossless compression can result in better visual quality at the expense of image size.

Lossless compression is turned OFF by default. You can switch on lossless compression using the parameter `lo-true` in the URL. This transformation parameter applies to both, PNG as well as WebP images.

For example:

[https://ik.imagekit.io/demo/img/tr:w-500,h-361,lo-true/default-image.jpg](https://ik.imagekit.io/demo/img/tr:w-500,h-361,lo-true/default-image.jpg)

The above URL delivers a losslessly compressed WebP image in Chrome. Adding the parameter `f-png` to the above URL results in a losslessly compressed PNG image.

Additionally, from the ImageKit.io dashboard, you can change the PNG compression mode from the Advanced Settings tab under Image Settings and set it to `Minimum` . This compression mode change from the dashboard is applicable only for PNG images.

You can completely turn off PNG image optimization by setting the PNG Compression mode to `None` from your dashboard. This is not recommended.

