# Configure origin

An origin is any web server or storage that currently stores and delivers your images. Currently, ImageKit.io supports any web server that can be accessed over HTTP\(S\), any S3 bucket, or a Cloudinary backup bucket.

It's very probable that your current setup utilizes multiple origins to serve images. For example, you might be using an NGINX server to deliver your product images and an S3 bucket for your marketing images. ImageKit.io allows you to add and manage multiple origins from a single account.

ImageKit.io supports the following types of origin:

1. [Amazon S3 bucket origin](amazon-s3-bucket-origin.md)
2. Any [web server origin](web-server-origin.md), for example - Magento, WordPress, etc.
3. [Azure Blob storage](azure-blob-storage.md)
4. [Google Storage](google-cloud-storage.md)
5. [Firebase Storage](firebase-storage.md)
6. [Cloudinary backup bucket](cloudinary-backup-bucket.md)

