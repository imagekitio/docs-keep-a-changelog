# Table of contents

* [What is ImageKit.io?](README.md)

## 📌Getting Started

* [Getting started](getting-started/quickstart-guides/README.md)
  * [React](getting-started/quickstart-guides/react.md)
  * [React Native](getting-started/quickstart-guides/react-native.md)
  * [Ruby on Rails](getting-started/quickstart-guides/ruby-on-rails.md)
  * [Vue.js](getting-started/quickstart-guides/vuejs.md)
* [How to use ImageKit.io?](getting-started/how-to-use-imagekit.io.md)

## 🔧External storage <a id="integration"></a>

* [Integration overview](integration/integration-overview.md)
* [How it works?](integration/how-it-works.md)
* [Configure origin](integration/configure-origin/README.md)
  * [Web server](integration/configure-origin/web-server-origin.md)
  * [Web proxy](integration/configure-origin/web-proxy.md)
  * [Amazon S3 bucket](integration/configure-origin/amazon-s3-bucket-origin.md)
  * [S3 Compatible External Storages](integration/configure-origin/s3-compatible-external-storages.md)
  * [Digital Ocean Spaces](integration/configure-origin/digital-ocean-spaces.md)
  * [Wasabi Storage](integration/configure-origin/wasabi-storage.md)
  * [Alibaba Object Storage Service](integration/configure-origin/alibaba-object-storage-service.md)
  * [Azure Blob storage](integration/configure-origin/azure-blob-storage.md)
  * [Google Cloud storage](integration/configure-origin/google-cloud-storage.md)
  * [Firebase storage](integration/configure-origin/firebase-storage.md)
  * [Cloudinary Backup Bucket](integration/configure-origin/cloudinary-backup-bucket.md)
* [URL-endpoints](integration/url-endpoints.md)
* [Integrate with your CDN](integration/integrate-with-your-cdn.md)
* [Multiple websites](integration/multiple-websites.md)
* [Testing](integration/testing.md)
* [404 - Not found troubleshooting](integration/404-not-found-error-troubleshooting.md)

## ImageKit.io storage <a id="media-library"></a>

* [Media Library](media-library/overview/README.md)
  * [Upload files](media-library/overview/upload-files.md)
  * [Search, update & delete files](media-library/overview/search-update-and-delete.md)
  * [Copy and move files](media-library/overview/copy-and-move-files.md)
  * [Image tagging and searching](media-library/overview/image-tags.md)
  * [Create folders](media-library/overview/folders.md)
  * [Copy and move folders](media-library/overview/copy-and-move-folders.md)
  * [Delete folder](media-library/overview/delete-folder.md)
  * [Custom focus area](media-library/overview/custom-focus-area.md)
  * [Edit image](media-library/overview/edit-image.md)

## ✨Features

* [Image Transformations](features/image-transformations/README.md)
  * [Resize, crop and other common transformations](features/image-transformations/resize-crop-and-other-transformations.md)
  * [Overlay](features/image-transformations/overlay.md)
  * [Image enhancement & color manipulation](features/image-transformations/image-enhancement-and-color-manipulation.md)
  * [Chained transformations](features/image-transformations/chained-transformations.md)
  * [Supported text font list](features/image-transformations/supported-text-font-list.md)
* [Image optimization](features/image-optimization/README.md)
  * [Automatic image format conversion](features/image-optimization/automatic-image-format-conversion.md)
  * [Quality Optimization](features/image-optimization/quality-optimization.md)
  * [Chroma Subsampling](features/image-optimization/chroma-subsampling.md)
  * [Data Saver Mode](features/image-optimization/data-saver-mode.md)
  * [Metadata, Color Profile and Orientation](features/image-optimization/metadata-color-profile-and-orientation.md)
  * [PNG Compression](features/image-optimization/png-compression.md)
* [Named Transformations](features/named-transformations.md)
* [Network-based image optimization](features/network-based-image-optimization.md)
* [Caches](features/caches.md)
* [Cache purging](features/cache-purging.md)
* [Security](features/security/README.md)
  * [Signed URLs](features/security/signed-urls.md)
  * [Private images](features/security/private-images.md)
* [Using custom domain](features/using-custom-domain.md)
* [SEO-friendly image URL](features/dynamic-seo-suffix.md)
* [Default Images](features/default-images.md)
* [Progressive JPEGs](features/progressive-jpegs.md)
* [Non-Image File Compression](features/non-image-file-compression.md)
* [Performance monitoring](features/performance-monitoring.md)
* [User access management](features/user-access-management.md)

## Platform guides

* [Magento](platform-guides/magento.md)
* [WordPress](platform-guides/wordpress.md)
* [Shopify](platform-guides/shopify.md)

## Sample projects

* [Upload widget](sample-projects/upload-widget/README.md)
  * [Uppy upload widget - Plain JS](sample-projects/upload-widget/uppy-upload-widget.md)

## 📙API Reference

* [API Introduction](api-reference/api-introduction/README.md)
  * [API keys](api-reference/api-introduction/api-keys.md)
  * [Authentication](api-reference/api-introduction/authentication.md)
  * [Rate Limits](api-reference/api-introduction/rate-limits.md)
  * [SDK](api-reference/api-introduction/sdk.md)
* [Upload file API](api-reference/upload-file-api/README.md)
  * [Server side file upload](api-reference/upload-file-api/server-side-file-upload.md)
  * [Client side file upload](api-reference/upload-file-api/client-side-file-upload.md)
* [Media API](api-reference/media-api/README.md)
  * [List and search files](api-reference/media-api/list-and-search-files.md)
  * [Get file details](api-reference/media-api/get-file-details.md)
  * [Update file details](api-reference/media-api/update-file-details.md)
  * [Add tags \(bulk\)](api-reference/media-api/add-tags-bulk.md)
  * [Remove tags \(bulk\)](api-reference/media-api/remove-tags-bulk.md)
  * [Delete file](api-reference/media-api/delete-file.md)
  * [Delete files \(bulk\)](api-reference/media-api/delete-files-bulk.md)
  * [Copy file](api-reference/media-api/copy-file.md)
  * [Move file](api-reference/media-api/move-file.md)
  * [Copy folder](api-reference/media-api/copy-folder.md)
  * [Move folder](api-reference/media-api/move-folder.md)
  * [Bulk job status](api-reference/media-api/copy-move-folder-status.md)
  * [Purge cache](api-reference/media-api/purge-cache.md)
  * [Purge cache status](api-reference/media-api/purge-cache-status.md)
* [Metadata API](api-reference/metadata-api/README.md)
  * [Get image metadata for uploaded media files](api-reference/metadata-api/get-image-metadata-for-uploaded-media-files.md)
  * [Get image metadata from remote URL](api-reference/metadata-api/get-image-metadata-from-remote-url.md)

