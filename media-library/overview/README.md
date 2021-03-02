---
description: >-
  A simple interface to upload, manage, search, and tag resources for efficient
  digital asset management in the cloud.
---

# Media Library

ImageKit.io provides highly available storage called the Media Library to all its users. It comes with a simple user interface to upload, tag, search and manage files, images, and folders.

You can also use the [media library widget](https://github.com/imagekit-developer/embeddable-media-library), which provides a way to easily integrate [ImageKit Media Library](/@imagekit-io/s/docs/~/drafts/-MUh12h2Hs3LTDxi7VGp/media-library/overview) into your CMS or any other web application. Using this, you can access all the assets stored in your Media Library from your existing CMS or application.

Learn more about how to use the media library widget.

{% page-ref page="../../sample-projects/embeddable-media-library-widget/" %}

The media library is built on top of AWS S3 and is co-located with core image processing servers in every location. We also replicate any image that is explicitly uploaded to the media library in two different geographic locations for data redundancy.

## Who Should Use the Media Library?

If you are not looking to spend time building your image storage or file upload and search functionality, ImageKit.io's Media Library is an excellent option for you. Not only is it highly performant storage, but it also helps you eliminate or reduce your server disk space usage with your current setup. Additionally, the user interface allows even non-technical members of your team have quick and easy access to images and static assets of your website and app.

One of the most common uses of the ImageKit.io's Media Library is the easy access to, and management of, a website's marketing assets. Every company communicates with its users through emails, banners, push notifications, and other channels. Often, such activities are executed by the marketing team, who require quick and easy ways to upload banners and new images, resize them, and get the URLs to access them. The Media Library is perfect for such a scenario, as the marketing team can create folders for each activity or campaign to keep all related assets and images within these folders. Further, each file is URL accessible and ready to use in push notifications, banners, and emails of your website and apps.

## Where is the ImageKit.io Media Library available geographically?

ImageKit.io has a globally distributed architecture to accommodate better performance, 100% uptime, and data redundancy across multiple geographic locations. On similar lines, the storage system that forms the basis of the Media Library is co-located with core processing servers. ImageKit.io storage is currently spread across 6 locations - USA West \(California\). USA East \(North Virginia\), Europe \(Frankfurt\), Asia \(Mumbai\), South East Asia \(Singapore\), and Australia \(Sydney\).

## Is the Media Library Secure?

Absolutely. ImageKit.io's Media Library is built on top of AWS S3, which is one of the most trusted storages used by businesses across the world. The images added to the media library can only be accessed through your ImageKit.io account. As an added layer of security, every image uploaded is backed up in another distinct geographic location for data redundancy.

For example, all our customers using the Media Library from Frankfurt, Germany will have their images and static assets backed up in USA East \(North Virginia\). ImageKit.io can then use this backup in case of performance degradation of the primary geographic storage, and/or can be used to restore old versions of images.

