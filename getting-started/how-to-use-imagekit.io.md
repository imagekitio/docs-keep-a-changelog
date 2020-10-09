# How to use ImageKit.io?

There are two ways to use ImageKit.io. Depending upon your needs, either you can plug ImageKit.io with your existing infrastructure without moving or re-uploading any existing images, or you can upload pictures in the ImageKit.io media library. Both options are always available and can be used together.

## 1. Integration with existing storage or server

ImageKit.io can be integrated into your existing infrastructure within minutes. 

{% hint style="success" %}
\*\*\*\*🧙♂ **Recommended if you don't want to move your existing images**  
If you have a running website or application, you don't need to move your images anywhere. Just configure your existing image origin and ImageKit.io will fetch the images on the first request and cache the response.
{% endhint %}

ImageKit.io supports the following types of origin:

1. [Amazon S3 bucket origin](../integration/configure-origin/amazon-s3-bucket-origin.md)
2. [S3-Compatible storages](../integration/configure-origin/s3-compatible-external-storages.md)
   1. [Wasabi Storage](../integration/configure-origin/wasabi-storage.md)
   2. [Ali Storage](../integration/configure-origin/alibaba-object-storage-service.md)
   3. [Digital Ocean Spaces](../integration/configure-origin/digital-ocean-spaces.md)
3. Any [web server origin](../integration/configure-origin/web-server-origin.md), for example - Magento, WordPress, etc.
4. [Web proxy](../integration/configure-origin/web-proxy.md)
5. [Azure Blob storage](../integration/configure-origin/azure-blob-storage.md)
6. [Google Storage](../integration/configure-origin/google-cloud-storage.md)
7. [Firebase Storage](../integration/configure-origin/firebase-storage.md)
8. [Cloudinary backup bucket](../integration/configure-origin/cloudinary-backup-bucket.md)

## 2. Using ImageKit storage \(Media Library\)

If you don't want to handle image upload to your own storage, then ImageKit's media library is excellent for you. The media library is an available unlimited internal storage provided by ImageKit.io to all its users. ImageKit.io media library is built on top of the Amazon S3, and it is co-located with core image processing servers in all the regions.

Media library allows you to:

1. Upload images via ImageKit.io dashboard, [upload API](../api-reference/upload-file-api/) or using SDKs.
2. [Create folders](../media-library/overview/folders.md) and [delete folders](../media-library/overview/delete-folder.md) to manage your images.
3. [Tag files](../media-library/overview/image-tags.md) for better organization.
4. [Search files](../media-library/overview/search-update-and-delete.md) by name, folder name, and tags.
5. Delete a file.

