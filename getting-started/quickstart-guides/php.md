---
description: >-
  Real-time image resizing, automatic optimization, and file uploading in PHP
  using ImageKit.io.
---

# PHP

[ImageKit's PHP SDK](https://github.com/imagekit-developer/imagekit-php) provides comprehensive yet straightforward asset upload, transformation, optimization, and delivery capabilities that you can implement seamlessly in your existing PHP application.

This is a quick start guide to show you how to integrate ImageKit in your PHP application. The code samples covered here are hosted on Github - [https://github.com/imagekit-developer/imagekit-php/tree/master/sample](https://github.com/imagekit-developer/imagekit-php/tree/master/sample)[.](https://github.com/imagekit-developer/imagekit-php/tree/master/sample)

This guide walks you through the following topics:

* [Setting up ImageKit PHP SDK](php.md#setting-up-imagekit-php-sdk)
* [Generating url for rendering images](php.md#generating-url-for-rendering-images)
* [Applying common image manipulations](php.md#applying-common-image-manipulations)
* [Adding overlays to images](php.md#adding-overlays-to-images-in-php)
* [Server-side file uploading](php.md#server-side-file-uploading)
* [ImageKit Media API](php.md#imagekit-media-api)
* [Utility Functions](php.md#utility-functions)

## Setting up ImageKit PHP SDK

#### Prerequisites

To use ImageKit PHP SDK, you must be using PHP version 5.6.0 or later with [JSON PHP Extension](https://www.php.net/manual/en/book.json.php) and [cURL PHP Extension](https://www.php.net/manual/en/book.curl.php) enabled.

#### Create a new Project

Let's create a dummy project called **sample** using composer in a folder.

```bash
composer init --name imagekit/sample --type project
```

It will prompt a few options. Select defaults by pressing enter.

#### Install imagekit/imagekit using Composer

```bash
composer require imagekit/imagekit
```

#### Initialize SDK

```php
// Import autoloader from vendor
// If not using PSR-4 is not configured in composer.json file for your project
require_once __DIR__ . '/vendor/autoload.php';

use ImageKit\ImageKit;

// For demonstration purposes, the documentation would use https://ik.imagekit.io/demo as urlEndpoint
$imageKit = new ImageKit(
    "{publicKey}",
    "{publicKey}",
    "{urlEndpoint}" 
);

```

* `urlEndpoint` is the required parameter. You can get the value of URL-endpoint from your ImageKit dashboard - [https://imagekit.io/dashboard#url-endpoints](https://imagekit.io/dashboard#url-endpoints).&#x20;
* `publicKey` and `privateKey` parameters are also required as these would be used for all ImageKit API, server-side upload, and generating token for client-side file upload. You can get these parameters from the developer section in your ImageKit dashboard - [https://imagekit.io/dashboard#developers](https://imagekit.io/dashboard#developers).

## Generating url for rendering images

#### URL for image with relative path&#x20;

```php
$imageURL = $imageKit->url(['path' => '/default-image.jpg']);

echo('Url : ' . $imageURL); 
// https://ik.imagekit.io/demo/default-image.jpg
```

![](../../.gitbook/assets/default-image.jpeg)

#### URL for image with relative path and custom URL-endpoint

```php
$imageURL = $imageKit->url([
      'urlEndpoint' => 'https://ik.imagekit.io/test',
      'path' => '/default-image.jpg',
]);

echo('Url : ' . $imageURL); 
// https://ik.imagekit.io/test/default-image.jpg
```

#### &#x20;URL for image with absolute url&#x20;

If you have an absolute image path coming from the backend API e.g. `https://www.custom-domain.com/default-image.jpg` then you can use `src` prop to load the image.

```php
$imageURL = $imageKit->url([
    'src' => 'https://example.com/default-image.jpg'
]);

echo('Url : ' . $imageURL); 
// https://ik.imagekit.io/demo/default-image.jpg
```

## Applying common image manipulations

This section covers the basics:

* [Resizing images ](php.md#resizing-images-in-php)
* [Quality manipulation](php.md#quality-manipulation)
* [Chained transformation](php.md#chained-transformation)
* [Sharpening and contrast transforms](php.md#sharpening-and-contrast-transformation)

The PHP SDK gives a name to each transformation parameter e.g. `height` for `h` and `width` for `w` parameter. It makes your code more readable.  See the [full list of supported transformations](https://github.com/imagekit-developer/imagekit-php#list-of-supported-transformations) in PHP SDK on Github.&#x20;

👉 If the property does not match any of the available options, it is added as it is.\
👉 Note that you can also use `h` and `w` parameter instead of `height` and `width`. See the complete list of transformations supported in ImageKit [here](../../features/image-transformations/resize-crop-and-other-transformations.md).

### Resizing images in PHP

Let's resize the image to width 400 and height 300.

```javascript
$imageURL = $imageKit->url(array(
    'path' => '/default-image.jpg',
    'transformation' => array(
        array(
            'height' => '300',
            'width' => '400',
        ),
    ),
));

echo('Url : ' . $imageURL);
// https://ik.imagekit.io/demo/tr:w-400,h-300/default-image.jpg

```

![400x300 image](<../../.gitbook/assets/image (66).png>)

### Quality manipulation

You can use the [quality parameter](../../features/image-transformations/resize-crop-and-other-transformations.md#quality-q) to change quality like this.

```javascript
$imageURL = $imageKit->url(array(
    'path' => '/default-image.jpg',
    'transformation' => array(
        array(
            'quality' => '40',
        ),
    ),
));

echo('Url : ' . $imageURL);
// https://ik.imagekit.io/demo/tr:q-40/default-image.jpg
```

![](<../../.gitbook/assets/image (67).png>)

### Chained transformation

You can pass more than one object in `transformation` parameter to chain these transformations sequentially.

For example, the following values will first resize the image to width 400, height 300, and then [rotate](../../features/image-transformations/resize-crop-and-other-transformations.md#rotate-rt) to 90 degrees.&#x20;

```javascript
$imageURL = $imageKit->url(array(
    'path' => '/default-image.jpg',
    
    // It means first resize the image to 400x300 and then rotate 90 degree
    'transformation' => array(
        array(
            'height' => '300',
            'width' => '400',
        ),
        array(
            'rotation' => '90'
        )
    ),
));

echo('Url : ' . $imageURL);
// https://ik.imagekit.io/demo/tr:w-400,h-300:rt-90/default-image.jpg
```

![](<../../.gitbook/assets/image (68).png>)

### Sharpening and contrast transformation

You can enhance images and manipulate colors like this.

```javascript
$imageURL = $imageKit->url(array(
    'path' => '/sample_image.jpg',
    
    // It means first resize the image to 400x300 and then rotate 90 degree
    'transformation' => array(
        [
            'format' => 'jpg',
            'progressive' => true,
            'effectSharpen' => '-',
            'effectContrast' => '1',
        ],
    ),
));

echo('Url : ' . $imageURL);
// https://ik.imagekit.io/demo/tr:f-jpg,pr-true,e-sharpen,e-contrast-1/sample_image.jpg
```

![](<../../.gitbook/assets/image (69).png>)

## Adding overlays to images in PHP

ImageKit.io allows you to can add [text](../../features/image-transformations/overlay.md#text-overlay) and [image overlay](../../features/image-transformations/overlay.md) dynamically.

For example:

```javascript
$imageURL = $imageKit->url(array(
    'path' => '/default-image.jpg',
    'urlEndpoint' => 'https://ik.imagekit.io/pshbwfiho'
    
    // It means first resize the image to 400x300 and then rotate 90 degree
    'transformation' => array(
        array(
            'height' => '300',
            'width' => '300',
            'overlayImage' => 'default-image.jpg',
            'overlaywidth' => '100',
            'overlayX' => '0',
            'overlayImageBorder' => '10_CDDC39' // 10px border of color CDDC39
        )
    ),
));

echo('Url : ' . $imageURL);
// https://ik.imagekit.io/pshbwfiho/tr:w-300,h-300,oi-default-image.jpg,ow-100,ox-0,oib-10_CDDC39/default-image.jpg
```

![](<../../.gitbook/assets/image (70).png>)

## Generating Signed URL for images in PHP

ImageKit.io PHP SDK can generate a secure [signed url](../../features/security/signed-urls.md) that allows you to protect your media assets.

For example:

```php
$imageURL = $imageKit->url([
    'path' => '/default-image.jpg',
    'transformation' => [
        [
            'height' => '300',
            'width' => '400',
        ],
    ],
    'signed' => true,
    'expireSeconds' => 300,
]);

echo('Signed url : ' . $imageURL);
// https://ik.imagekit.io/demo/tr:h-300,w-400/default-image.jpg?ik-t=1632816299&ik-s=ffe7bc9445f95b4e33b4fb25ac504557a4ae03fc
```

You can manage [security settings](../../features/security/#restricting-unsigned-urls) from the dashboard to prevent unsigned URLs usage. In that case, if the URL doesn't have signature `ik-s` parameter or the signature is invalid, ImageKit will return a forbidden error instead of an actual image.

## Server-side File Upload

The SDK provides a simple interface using the `$imageKit->upload()` or `$imageKit->uploadFile()` method to upload files to the [ImageKit Media Library](https://imagekit.io/dashboard/media-library). 

- [Check all the supported file types and extensions](https://docs.imagekit.io/api-reference/upload-file-api#allowed-file-types-for-uploading).
- [Check all the supported parameters and details](https://docs.imagekit.io/api-reference/upload-file-api/server-side-file-upload).

#### Basic Usage
```php
$uploadFile = $imageKit->upload([
    'file' => 'your_file',              //  required, "binary","base64" or "file url"
    'fileName' => 'your_file_name.jpg', //  required
]);
```
#### Response
```json
{
    "err": null,
    "success": {
        "fileId": "6286329dfef1b033aee60211",
        "name": "your_file_name_S-PgGysnR.jpg",
        "size": 94466,
        "versionInfo": {
            "id": "6286329dfef1b033aee60211",
            "name": "Version 1"
        },
        "filePath": "/your_file_name_S-PgGysnR.jpg",
        "url": "https://ik.imagekit.io/demo/your_file_name_S-PgGysnR.jpg",
        "fileType": "image",
        "height": 640,
        "width": 960,
        "thumbnailUrl": "https://ik.imagekit.io/demo/tr:n-ik_ml_thumbnail/your_file_name_S-PgGysnR.jpg",
        "tags": [],
        "AITags": null,
        "customMetadata": { },
        "extensionStatus": {}
    }
}
```
#### Optional Parameters
```php
// Set of optional parameters
$uploadOptions = [
    "useUniqueFileName" => true,            // true|false
    "tags" => implode(",",["abd", "def"]),  // max: 500 chars
    "folder" => "/sample-folder",           
    "isPrivateFile" => false,               // true|false
    "customCoordinates" => implode(",", ["10", "10", "100", "100"]),    // max: 500 chars
    "responseFields" => implode(",", ["tags", "customMetadata"]),
    "extensions" => json_encode([       
        [
            "name" => "remove-bg",
            "options" => [  // refer https://docs.imagekit.io/extensions/overview
                "add_shadow" => true
            ]
        ]
    ]),
    "webhookUrl" => "https://example.com/webhook",
    "overwriteFile" => true,        // in case of false useUniqueFileName should be true
    "overwriteAITags" => true,      // set to false in order to preserve overwriteAITags
    "overwriteTags" => true,
    "overwriteCustomMetadata" => true,
    // "customMetadata" => json_encode([
    //         "SKU" => "VS882HJ2JD",
    //         "price" => 599.99,
    // ])
];

// Attempt File Uplaod
$uploadFile = $imageKit->upload([
    'file' => 'your_file',                  //  required, "binary","base64" or "file url"
    'fileName' => 'your_file_name.jpg',     //  required
    'options' => $uploadOptions             // optional
]);
```  


## ImageKit Media API

The SDK provides a simple interface for all the following [Media APIs](https://docs.imagekit.io/api-reference/media-api) to manage your files.

### 1. List and Search Files

This API can list all the uploaded files and folders in your [ImageKit.io](https://docs.imagekit.io/api-reference/media-api) media library.

#### Basic Usage
```php
$listFiles = $imageKit->listFiles();
```
#### Response
```json
[
    {
        "fileId": "598821f949c0a938d57563bd",
        "type": "file",
        "name": "file1.jpg",
        "filePath": "/images/products/file1.jpg",
        "tags": ["t-shirt", "round-neck", "sale2019"],
        "AITags": [
            {
                "name": "Shirt",
                "confidence": 90.12,
                "source": "google-auto-tagging"
            },
            /* ... more googleVision tags ... */
        ],
        "versionInfo": {
            "id": "598821f949c0a938d57563bd",
            "name": "Version 1"
        },
        "isPrivateFile": false,
        "customCoordinates": null,
        "url": "https://ik.imagekit.io/your_imagekit_id/images/products/file1.jpg",
        "thumbnail": "https://ik.imagekit.io/your_imagekit_id/tr:n-media_library_thumbnail/images/products/file1.jpg",
        "fileType": "image",
        "mime": "image/jpeg",
        "width": 100,
        "height": 100,
        "size": 100,
        "hasAlpha": false,
        "customMetadata": {
            "brand": "Nike",
            "color": "red"
        },
        "createdAt": "2019-08-24T06:14:41.313Z",
        "updatedAt": "2019-08-24T06:14:41.313Z"
    },
    ...more items
]
```
#### Applying Filters
Filter out the files with an object specifying the parameters. 
```php
$listFiles = $imageKit->listFiles([
    "type" => "file",           // file, file-version or folder
    "sort" => "ASC_CREATED",    
    "path" => "/",              // folder path
    "fileType" => "all",        // all, image, non-image
    "limit" => 10,              // min:1, max:1000
    "skip" => 0,                // min:0
    "searchQuery" => 'size < "20kb"',
]);
```

#### Advance Search
In addition, you can fine-tune your query by specifying various filters by generating a query string in a Lucene-like syntax and provide this generated string as the value of the `searchQuery`.
```php
$listFiles = $imageKit->listFiles([
    "searchQuery" => '(size < "1mb" AND width > 500) OR (tags IN ["summer-sale","banner"])',
]);
```
Detailed documentaion can be found here for [Advance Search Queries](https://docs.imagekit.io/api-reference/media-api/list-and-search-files#advanced-search-queries).

### 2. Get File Details

This API can get you all the details and attributes of the current version of the file.

#### Basic Usage
```php
$getFileDetails = $imageKit->getFileDetails('file_id');
```
#### Response
```json
{
    "fileId": "598821f949c0a938d57563bd",
    "type": "file",
    "name": "file1.jpg",
    "filePath": "/images/products/file1.jpg",
    "tags": ["t-shirt", "round-neck", "sale2019"],
    "AITags": [
        {
            "name": "Shirt",
            "confidence": 90.12,
            "source": "google-auto-tagging"
        },
        /* ... more googleVision tags ... */
    ],
    "versionInfo": {
            "id": "598821f949c0a938d57563bd",
            "name": "Version 1"
    },
    "isPrivateFile": false,
    "customCoordinates": null,
    "url": "https://ik.imagekit.io/your_imagekit_id/images/products/file1.jpg",
    "thumbnail": "https://ik.imagekit.io/your_imagekit_id/tr:n-media_library_thumbnail/images/products/file1.jpg",
    "fileType": "image",
    "mime": "image/jpeg",
    "width": 100,
    "height": 100,
    "size": 100,
    "hasAlpha": false,
    "customMetadata": {
        "brand": "Nike",
        "color": "red"
    },
    "createdAt": "2019-08-24T06:14:41.313Z",
    "updatedAt": "2019-08-24T06:14:41.313Z"
}
```

### 3. Get File Version Details

This API can get you all the details and attributes for the provided version of the file.`versionID` can be found in the following APIs as `id` within the `versionInfo` parameter:
- [Server-side File Upload API](#server-side-file-upload).
- [List & Search File API](#1-list-and-search-files)
- [Get File Details API](#2-get-file-details)

#### Basic Usage
```php
$getFileVersionDetails = $imageKit->getFileVersionDetails('file_id','version_id');
```
#### Response
```json
{
    "fileId": "598821f949c0a938d57563bd",
    "type": "file",
    "name": "file1.jpg",
    "filePath": "/images/products/file1.jpg",
    "tags": ["t-shirt", "round-neck", "sale2019"],
    "AITags": [
        {
            "name": "Shirt",
            "confidence": 90.12,
            "source": "google-auto-tagging"
        },
        /* ... more googleVision tags ... */
    ],
    "versionInfo": {
            "id": "598821f949c0a938d57563bd",
            "name": "Version 1"
    },
    "isPrivateFile": false,
    "customCoordinates": null,
    "url": "https://ik.imagekit.io/your_imagekit_id/images/products/file1.jpg",
    "thumbnail": "https://ik.imagekit.io/your_imagekit_id/tr:n-media_library_thumbnail/images/products/file1.jpg",
    "fileType": "image",
    "mime": "image/jpeg",
    "width": 100,
    "height": 100,
    "size": 100,
    "hasAlpha": false,
    "customMetadata": {
        "brand": "Nike",
        "color": "red"
    },
    "createdAt": "2019-08-24T06:14:41.313Z",
    "updatedAt": "2019-08-24T06:14:41.313Z"
}
```

### 4. Get File Versions

This API can get you all the versions of the file.

#### Basic Usage
```php
$getFileVersions = $imageKit->getFileVersions('file_id');
```
#### Response
```json
[
    {
        "fileId": "598821f949c0a938d57563bd",
        "type": "file-version",
        "name": "file1.jpg",
        "filePath": "/images/products/file1.jpg",
        "tags": ["t-shirt", "round-neck", "sale2019"],
        "AITags": [
            {
                "name": "Shirt",
                "confidence": 90.12,
                "source": "google-auto-tagging"
            },
            /* ... more googleVision tags ... */
        ],
        "versionInfo": {
                "id": "697821f849c0a938d57563ce",
                "name": "Version 2"
        },
        "isPrivateFile": false,
        "customCoordinates": null,
        "url": "https://ik.imagekit.io/your_imagekit_id/images/products/file1.jpg?ik-obj-version=bREnN9Z5VQQ5OOZCSvaXcO9SW.su4QLu",
        "thumbnail": "https://ik.imagekit.io/your_imagekit_id/tr:n-media_library_thumbnail/images/products/file1.jpg?ik-obj-version=bREnN9Z5VQQ5OOZCSvaXcO9SW.su4QLu",
        "fileType": "image",
        "mime": "image/jpeg",
        "width": 100,
        "height": 100,
        "size": 100,
        "hasAlpha": false,
        "customMetadata": {
            "brand": "Nike",
            "color": "red"
        },
        "createdAt": "2019-08-24T06:14:41.313Z",
        "updatedAt": "2019-09-24T06:14:41.313Z"
    },
    ...more items
]
```

### 5. Update File Details

Update file details such as tags, customCoordinates attributes, remove existing AITags and apply [extensions](https://docs.imagekit.io/extensions/overview) using Update File Details API. This operation can only be performed on the current version of the file.

#### Basic Usage
```php
// Update parameters
$updateData = [
        "removeAITags" => "all",    // "all" or ["tag1","tag2"]
        "webhookUrl" => "https://example.com/webhook",
        "extensions" => [       
            [
                "name" => "remove-bg",
                "options" => [  // refer https://docs.imagekit.io/extensions/overview
                    "add_shadow" => true
                ]
            ],
            [
                "name" => "google-auto-tagging",
            ]
        ],
        "tags" => ["tag1", "tag2"],
        "customCoordinates" => "10,10,100,100",
        // "customMetadata" => [
        //     "SKU" => "VS882HJ2JD",
        //     "price" => 599.99,
        // ]
];

// Attempt Update
$updateFileDetails = $imageKit->updateFileDetails(
    'file_id',
    $updateData
);
```
#### Response
```json
// This example response is after extensions are applied in the update API.
{
    "fileId" : "598821f949c0a938d57563bd",
    "type": "file",
    "name": "file1.jpg",
    "filePath": "/images/products/file1.jpg",
    "tags": ["t-shirt","round-neck","sale2019"],
    "AITags": [],
    "versionInfo": {
            "id": "598821f949c0a938d57563bd",
            "name": "Version 1"
    },
    "isPrivateFile" : false,
    "customCoordinates" : null,
    "url": "https://ik.imagekit.io/your_imagekit_id/images/products/file1.jpg",
    "thumbnail": "https://ik.imagekit.io/your_imagekit_id/tr:n-media_library_thumbnail/images/products/file1.jpg",
    "fileType": "image",
    "mime": "image/jpeg",
    "width": 100,
    "height": 100,
    "size": 100,
    "hasAlpha": false,
    "customMetadata": {
        "brand": "Nike",
        "color": "red"
    },
    "extensionStatus": {
        "remove-bg": "pending",
        "google-auto-tagging": "success"
    },
    "createdAt": "2019-08-24T06:14:41.313Z",
    "updatedAt": "2019-08-24T06:14:41.313Z"
}
```

### 6. Add Tags (Bulk) API

Add tags to multiple files in a single request. The method accepts an array of `fileIds` of the files and an array of `tags` that have to be added to those files.

#### Basic Usage
```php
$fileIds = ['file_id1','file_id2'];
$tags = ['image_tag_1', 'image_tag_2'];

$bulkAddTags = $imageKit->bulkAddTags($fileIds, $tags);
```
#### Response
```json
{
    "err": null,
    "success": {
        "successfullyUpdatedFileIds": [
            "5e21880d5efe355febd4bccd",
            "5e1c13c1c55ec3437c451403"
        ]
    }
}
```

### 7. Remove Tags (Bulk) API

Remove tags from multiple files in a single request. The method accepts an array of `fileIds` of the files and an array of `tags` that have to be removed from those files.

#### Basic Usage
```php
$fileIds = ['file_id1','file_id2'];
$tags = ['image_tag_1', 'image_tag_2'];

$bulkRemoveTags = $imageKit->bulkRemoveTags($fileIds, $tags);
```
#### Response
```json
{
    "err": null,
    "success": {
        "successfullyUpdatedFileIds": [
            "5e21880d5efe355febd4bccd",
            "5e1c13c1c55ec3437c451403"
        ]
    }
}
```

### 8. Remove AI Tags (Bulk) API

Remove AI tags from multiple files in a single request. The method accepts an array of `fileIds` of the files and an array of `AITags` that have to be removed from those files.

#### Basic Usage
```php
$fileIds = ['file_id1','file_id2'];
$AITags = ['image_AITag_1', 'image_AITag_2'];

$bulkRemoveTags = $imageKit->bulkRemoveTags($fileIds, $AITags);
```
#### Response
```json
{
    "err": null,
    "success": {
        "successfullyUpdatedFileIds": [
            "5e21880d5efe355febd4bccd",
            "5e1c13c1c55ec3437c451403"
        ]
    }
}
```



### Delete file

Delete a file as per the [API documentation here](https://docs.imagekit.io/api-reference/media-api/delete-file). The method accepts the file ID of the file that has to be deleted.

```php
$imageKit->deleteFile($fileId);
```

### Delete files bulk

Deletes multiple files and all their transformations as per the [API documentation here](https://docs.imagekit.io/api-reference/media-api/delete-files-bulk). The method accepts the array of file IDs that have to be deleted.

```php
$imageKit->bulkFileDeleteByIds(array(
    "fileIds" => array("file_id_1", "file_id_2", ...)
));
```

### Copy file

This will copy a file from one location to another as per [API documentation here](https://docs.imagekit.io/api-reference/media-api/copy-file). This method accepts the source file's path and destination folder path.

```php
$imagekit->copyFile('/source/path', '/destination/path');
```

### Move file

This will move a file from one location to another as per [API documentation here](https://docs.imagekit.io/api-reference/media-api/move-file). This method accepts the source file's path and destination folder path.

```php
$imagekit->moveFile('/source/path', '/destination/path');
```

### Rename file

This will rename an already existing file in the media library as per [API Documentation here](../../api-reference/media-api/rename-file.md). This method accepts the source file's path, the new name of the file, and an optional boolean parameter to purge the CDN cache after renaming.&#x20;

{% hint style="info" %}
**Limits on purge**\
****Purging has account-level limits. Please refer to [purging limits](../../api-reference/media-api/purge-cache.md#limitations) before using this parameter.
{% endhint %}

```php
// Purge Cache would default to false
$imagekit->renameFile('/filePath', 'newFileName');

// Purge Cache explicitly set to false
$imagekit->renameFile('/filePath', 'newFileName', false);

// Purge Cache explicitly set to true
$imagekit->renameFile('/filePath', 'newFileName', true);
```

### Create folder

This will create a new folder as per [API documentation here](https://docs.imagekit.io/api-reference/media-api/create-folder). This method accepts the folder name and parent folder path.

```php
$imagekit->createFolder('folderName', '/parentFolderPath');
```

### **Delete folder**

This will delete the specified folder and all nested files & folders as per [API documentation here](https://docs.imagekit.io/api-reference/media-api/delete-folder). This method accepts the full path of the folder that is to be deleted.

```php
$imagekit->deleteFolder('/folderPath');
```

### Copy folder

This will copy one folder into another as per [API documentation here](https://docs.imagekit.io/api-reference/media-api/copy-folder). This method accepts the source folder's path and destination folder path.

```php
$imagekit->copyFolder('/source/path', '/destination/path');
```

### Move folder

This will move one folder into another as per [API documentation here](https://docs.imagekit.io/api-reference/media-api/move-folder). This method accepts the source folder's path and destination folder path.

```php
$imagekit->moveFolder('/source/path', '/destination/path');
```

### Get bulk job status

This allows us to get a bulk operation status e.g. copy or move folder as per [API documentation here](https://docs.imagekit.io/api-reference/media-api/copy-move-folder-status). This method accepts `jobId` that is returned by copy and move folder operations.

```php
$imagekit->getBulkJobStatus('jobId');
```

### Purge cache

Programmatically issue a cache clear request as per the [API documentation here](https://docs.imagekit.io/api-reference/media-api/purge-cache). Accepts the full URL of the file for which the cache has to be cleared.

```php
$imagekit->purgeCache('image_url');
```

### Purge cache status

Get the purge cache request status using the request ID returned when a purge cache request gets submitted as per the [API documentation here](https://docs.imagekit.io/api-reference/media-api/purge-cache-status)

```php
$imagekit->getPurgeCacheStatus('request_id');
```

### **Get file metadata**

Accepts the file ID and fetches the metadata as per the [API documentation here](https://docs.imagekit.io/api-reference/metadata-api/get-image-metadata-for-uploaded-media-files).

```php
$imageKit->getFileMetaData("file_id");
```

You can also** **get metadata of the image using the absolute image URL. The image URL should be powered by ImageKit and accessible via your account.

```php
$imageKit->getFileMetadataFromRemoteURL("imagekit_remote_url");
```

## Utility Functions

We have included the following commonly used utility functions in this SDK.

### Authentication parameter generation

In case you are looking to implement client-side file upload, you are going to need a `token`, `expiry` timestamp and a valid `signature` for that upload. The SDK provides a simple method that you can use in your code to generate these authentication parameters for you.

{% hint style="danger" %}
The Private API Key should never be exposed in any client-side code. You must always generate these authentication parameters on the server-side.
{% endhint %}

```
$imageKit->getAuthenticationParameters($token = "", $expire = 0);
```

It will return

```php
stdClass Object
(
    [token] => unique_token,
    [expire] => valid_expiry_timestamp,
    [signature] => generated_signature
)
```

Both the `token` and `expire` parameters are optional. If not specified, the SDK generates a random token and also generates a valid expiry timestamp internally. The value of the `token` and `expire` used to generate the signature are always returned in the response, no matter if they are provided as an input to this method or not.

### Distance calculation between two pHash values

Perceptual hashing allows you to construct a hash value that uniquely identifies an input image based on the contents of an image. [ImageKit.io metadata API](https://docs.imagekit.io/api-reference/metadata-api) returns the pHash value of an image in the response. You can use this value to find a duplicate (or similar) image by calculating the distance between the pHash value of the two images.

This SDK exposes `pHashDistance` function to calculate the distance between two pHash values. It accepts two pHash hexadecimal strings and returns a numeric value indicative of the level of difference between the two images.

```php
  $imageKit->pHashDistance($firstHash ,$secondHash);
```

#### Distance calculation examples

```php
$imageKit->pHashDistance('f06830ca9f1e3e90', 'f06830ca9f1e3e90');
// output: 0 (same image)

$imageKit->pHashDistance('2d5ad3936d2e015b', '2d6ed293db36a4fb');
// output: 17 (similar images)

$imageKit->pHashDistance('a4a65595ac94518b', '7838873e791f8400');
// output: 37 (dissimilar images)
```
