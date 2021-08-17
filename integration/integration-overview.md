---
description: >-
  ImageKit.io integration with your existing storage like Amazon S3, Azure,
  Google storage or Ngnix server.
---

# Integration overview

You can integrate ImageKit.io with your existing infrastructure in a few minutes. Here are the steps:

## Step 1: Configure the origin

ImageKit.io supports the following type of origin:

1. [Amazon S3 bucket origin](configure-origin/amazon-s3-bucket-origin.md)
2. [S3-Compatible storages](configure-origin/s3-compatible-external-storages.md)
   1. [Wasabi Storage](configure-origin/wasabi-storage.md)
   2. [Ali Storage](configure-origin/alibaba-object-storage-service.md)
   3. [Digital Ocean Spaces](configure-origin/digital-ocean-spaces.md)
3. Any [web server origin](configure-origin/web-server-origin.md), for example - Magento, WordPress, etc.
4. [Web proxy](configure-origin/web-proxy.md)
5. [Azure Blob storage](configure-origin/azure-blob-storage.md)
6. [Google Storage](configure-origin/google-cloud-storage.md)
7. [Firebase Storage](configure-origin/firebase-storage.md)
8. [Cloudinary backup bucket](configure-origin/cloudinary-backup-bucket.md)

## Step 2: Access the image through ImageKit.io URL endpoint

When you sign up, a default [URL-endpoint](url-endpoints.md) is created in your ImageKit.io dashboard.

The default URL endpoint is always `https://ik.imagekit.io/your_imagekit_id`.

Let's assume the original image URL is `https://www.example.com/rest-of-the-path.jpg`. The same image can be accessed through ImageKit.io URL-endpoint now - `https://ik.imagekit.io/your_imagekit_id/rest-of-the-path.jpg`

We just replaced the old base URL `https://www.example.com` with the new ImageKit.io URL-endpoint, i.e. `https://ik.imagekit.io/your_imagekit_id`.

{% hint style="info" %}
🧙♂**Tip:** You can also use a [custom domain](../testing-and-infrastructure-setup/using-custom-domain-name.md) like `images.example.com`. But in this documentation, we will stick with the `https://ik.imagekit.io/your_imagekit_id` format. Learn more about how to [use a custom domain](../testing-and-infrastructure-setup/using-custom-domain-name.md).
{% endhint %}

If you get a "Not found" error while accessing the image, check out this [troubleshooting guide](../limits-and-troubleshooting/404-not-found-error-troubleshooting.md).

## Step 3: Try optimization and resizing

Images fetched through ImageKit.io are automatically optimized and converted to the appropriate format. You can resize and transform an image by adding URL parameters. For example:

**Image with width 300px and height is adjusted automatically to preserve aspect ratio**

`https://ik.imagekit.io/your_imagekit_id/rest-of-the-path.jpg?tr=w-300`

 or `https://ik.imagekit.io/your_imagekit_id/tr:w-300/rest-of-the-path.jpg`

ImageKit.io supports multiple image transformations. [Learn more](../features/image-transformations/).

## Step 4: Go live on the production website

After the above URLs are working and you have tested the transformations, start using ImageKit.io URL endpoint in your application to accelerate image loading.

**Get started with our SDKs:**

{% page-ref page="../api-reference/api-introduction/sdk.md" %}

**Learn about real-time resizing:**

{% page-ref page="../features/image-transformations/" %}

