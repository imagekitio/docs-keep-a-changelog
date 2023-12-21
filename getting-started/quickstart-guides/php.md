---
description: >-
  Real-time image resizing, automatic optimization, and file uploading in PHP
  using ImageKit.io.
---

# PHP

[ImageKit's PHP SDK](https://github.com/imagekit-developer/imagekit-php) provides comprehensive yet straightforward asset upload, transformation, optimization, and delivery capabilities that you can implement seamlessly in your existing PHP application.

This quick start guide shows you how to integrate ImageKit into your PHP application. The code samples covered here are hosted on Github - [https://github.com/imagekit-developer/imagekit-php/tree/master/sample](https://github.com/imagekit-developer/imagekit-php/tree/master/sample)[.](https://github.com/imagekit-developer/imagekit-php/tree/master/sample)

This guide walks you through the following topics:

* [Setting up ImageKit PHP SDK](php.md#setting-up-imagekit-php-sdk)
* [Generating url for rendering images](php.md#generating-url-for-rendering-images)
* [Signed URL & Image Transformations](php.md#applying-chained-transformations-common-image-manipulations--signed-url)
* [Adding overlays](php.md#5.-adding-overlays)
* [Server-side file uploading](php.md#server-side-file-upload)
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

## Quick Examples

#### Create an ImageKit Instance

```php
// Import autoloader from vendor
// If not using PSR-4 is not configured in composer.json file for your project
require_once __DIR__ . '/vendor/autoload.php';

use ImageKit\ImageKit;  
  
// For demonstration purposes, the documentation would use https://ik.imagekit.io/demo as urlEndpoint
$imageKit = new ImageKit(
    "publicKey",
    "privateKey",
    "urlEndpoint"
);
```
* `publicKey` and `privateKey` parameters are required as these would be used for all ImageKit API, server-side upload, and generating token for client-side file upload. You can get these parameters from the developer section in your ImageKit dashboard - [https://imagekit.io/dashboard/developer/api-keys](https://imagekit.io/dashboard/developer/api-keys).
* `urlEndpoint` is also a required parameter. You can get the value of URL-endpoint from your ImageKit dashboard - [https://imagekit.io/dashboard/url-endpoints](https://imagekit.io/dashboard/url-endpoints).&#x20;

#### URL Generation
```php
// For URL Generation
$imageURL = $imageKit->url(
    [
        'path' => '/default-image.jpg',
    ]
);
echo $imageURL;
// https://ik.imagekit.io/demo/default-image.jpg
```

#### File Upload
```php
// For File Upload
$uploadFile = $imageKit->uploadFile([
    'file' => 'file-url',
    'fileName' => 'new-file'
]);
```

#### Response Structure
Following is the response for [Server Side File Upload API](../../api-reference/upload-file-api/server-side-file-upload.md#response-code-and-structure-json)
```JSON
{
    "error": null,
    "result": {
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
    },
    "responseMetadata":{
        "headers":{
            "access-control-allow-origin": "*",
            "x-ik-requestid": "e98f2464-2a86-4934-a5ab-9a226df012c9",
            "content-type": "application/json; charset=utf-8",
            "content-length": "434",
            "etag": "W/"1b2-reNzjRCFNt45rEyD7yFY/dk+Ghg"",
            "date": "Thu, 16 Jun 2022 14:22:01 GMT",
            "x-request-id": "e98f2464-2a86-4934-a5ab-9a226df012c9"
        },
        "raw":{
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
        },
        "statusCode":200
    }
}
```

## Generating URL for rendering images
ImageKit provides inbuilt media storage and integration with external origins. Refer to the [documentation](https://docs.imagekit.io/integration/url-endpoints) to learn more about URL endpoints and external [Image origins](https://docs.imagekit.io/integration/configure-origin) supported by ImageKit.  

### Using Image path and image hostname or endpoint 
  
This method allows you to create a URL using the image's path and the ImageKit URL endpoint (urlEndpoint) you want to use to access the image.   
  
#### Example
```php  
$imageURL = $imageKit->url(
    [
        'path' => '/default-image.jpg', 
        'transformation' => [
            [
                'height' => '300', 
                'width' => '400'
            ]
        ]
    ]
);
```  

#### Response

```  
https://ik.imagekit.io/demo/tr:h-300,w-400/default-image.jpg 
```  


### Using full image URL
This method allows you to add transformation parameters to an absolute ImageKit-powered URL. This method should be used if you have the absolute URL stored in your database.

#### Example
```php  
$imageURL = $imageKit->url([
    'src' => 'https://example.com/default-image.jpg',
    'transformation' => [
        [
            'height' => '300',
            'width' => '400'
        ]
    ]
]);
```  

#### Response
```
https://example.com/tr:h-300,w-400/default-image.jpg  
```  

The `$imageKit->url()` method accepts the following parameters.

| Option                | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |  
| :-------------------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |  
| urlEndpoint           | Optional. The base URL is to be appended before the path of the image. If not specified, the URL Endpoint specified at the time of SDK initialization is used. For example, https://ik.imagekit.io/your_imagekit_id/endpoint/                                                                                                                                                                                                                                                                                                                                                               |  
| path                  | Conditional. This is the path on which the image exists. For example, `/path/to/image.jpg`. Either the `path` or `src` parameter needs to be specified for URL generation.                                                                                                                                                                                                                                                                                                                                                                                                                |  
| src                   | Conditional. This is the complete URL of an image already mapped to ImageKit. For example, `https://ik.imagekit.io/your_imagekit_id/endpoint/path/to/image.jpg`. Either the `path` or `src` parameter needs to be specified for URL generation.                                                                                                                                                                                                                                                                                                                                           |  
| transformation        | Optional. An array of objects specifying the transformation to be applied in the URL. The transformation name and the value should be specified as a key-value pair in the object. Different steps of a [chained transformation](https://docs.imagekit.io/features/image-transformations/chained-transformations) can be specified as different objects of the array. The complete [List of supported transformations](#list-of-supported-transformations) in the SDK and some examples of using them are given later. If you use a transformation name that is not specified in the SDK, it gets applied as it is in the URL. |  
| transformationPosition | Optional. The default value is `path` which places the transformation string as a path parameter in the URL. It can also be specified as `query`, which adds the transformation string as the query parameter `tr` in the URL. If you use the `src` parameter to create the URL, the transformation string is always added as a query parameter.                                                                                                                                                                                                                                                 |  
| queryParameters       | Optional. These are the other query parameters that you want to add to the final URL. These can be any query parameters and are not necessarily related to ImageKit. Especially useful if you want to add some versioning parameters to your URLs.                                                                                                                                                                                                                                                                                                                                           |  
| signed                | Optional. Boolean. The default value is `false`. If set to `true`, the SDK generates a signed image URL adding the image signature to the image URL.                                                                                                                                                                                                                                                                                                              |  
| expireSeconds         | Optional. Integer. It is used along with the `signed` parameter. It specifies the time in seconds from now when the signed URL will expire. If specified, the URL contains the expiry timestamp in the URL, and the image signature is modified accordingly.                                                                                                                                                
                                             

### Applying Chained Transformations, Common Image Manipulations & Signed URL

This section covers the basics:

* [Chained Transformations as a query parameter](#1-chained-transformations-as-a-query-parameter)
* [Image Enhancement & Color Manipulation](#2-image-enhancement-and-color-manipulation)
* [Resizing images](#3-resizing-images)
* [Quality manipulation](#4-quality-manipulation)
* [Adding overlays](#5-adding-overlays)
* [Signed URL](#6-signed-url)


The PHP SDK gives a name to each transformation parameter e.g. `height` for `h` and `width` for `w` parameter. It makes your code more readable. See the [Full list of supported transformations](#list-of-supported-transformations).

👉 If the property does not match any of the available options, it is added as it is.\ e.g
```PHP
[
    'effectGray' => 'e-grayscale'
]
// and
[
    'e-grayscale' => ''
]
// works the same
```
👉 Note that you can also use the `h` and `w` parameters instead of `height` and `width`. 

For more examples check the [demo application](https://github.com/imagekit-developer/imagekit-php/tree/master/sample).

### 1. Chained Transformations as a query parameter

#### Example
```php  
$imageURL = $imageKit->url([
    'path' => '/default-image.jpg',
    'urlEndpoint' => 'https://ik.imagekit.io/demo/', 
    'transformation' => [
        [
            'height' => '300',
            'width' => '400'
        ],
        [
            'rotation' => 90
        ],
    ], 
    'transformationPosition' => 'query'
]);
```  
#### Response
```  
https://ik.imagekit.io/demo/default-image.jpg?tr=h-300,w-400:rt-90
```  
![](<../../.gitbook/assets/image (68).png>)

### 2. Image enhancement and color manipulation

Some transformations like [Contrast stretch](https://docs.imagekit.io/features/image-transformations/image-enhancement-and-color-manipulation#contrast-stretch-e-contrast) , [Sharpen](https://docs.imagekit.io/features/image-transformations/image-enhancement-and-color-manipulation#sharpen-e-sharpen) and [Unsharp mask](https://docs.imagekit.io/features/image-transformations/image-enhancement-and-color-manipulation#unsharp-mask-e-usm) can be added to the URL with or without any other value. To use such transforms without specifying a value, specify the value as "-" in the transformation object. Otherwise, specify the value that you want to be added to this transformation.

#### Example
```php  
$imageURL = $imageKit->url([
    'src' => 'https://ik.imagekit.io/demo/default-image.jpg', 
    'transformation' => 
    [
        [
            'format' => 'jpg', 
            'progressive' => true,
            'effectSharpen' => '-', 
            'effectContrast' => '1'
        ]
    ]
]);
```  
#### Response
```  
https://ik.imagekit.io/demo/tr:f-jpg,pr-true,e-sharpen,e-contrast-1/default-image.jpg 
```  
![](<../../.gitbook/assets/image (69).png>)

### 3. Resizing images
Let's resize the image to a width of 400 and a height of 300.
Check detailed instructions on [Resize, Crop, and Other Common Transformations](https://docs.imagekit.io/features/image-transformations/resize-crop-and-other-transformations)

#### Example
```php
$imageURL = $imageKit->url(array(
    'path' => '/default-image.jpg',
    'transformation' => [
        [
            'height' => '300',
            'width' => '400',
        ]
    ]
));
```
#### Response
```
https://ik.imagekit.io/demo/tr:w-400,h-300/default-image.jpg
```
![400x300 image](<../../.gitbook/assets/image (66).png>)

### 4. Quality manipulation
You can use the [Quality Parameter](https://docs.imagekit.io/features/image-transformations/resize-crop-and-other-transformations#quality-q) to change quality like this.

#### Example
```php
$imageURL = $imageKit->url(array(
    'path' => '/default-image.jpg',
    'transformation' => [
        [
            'quality' => '40',
        ]
    ]
));
```

#### Response
```
https://ik.imagekit.io/demo/tr:q-40/default-image.jpg
```

![](<../../.gitbook/assets/image (67).png>)


### 5. Adding overlays

ImageKit.io enables you to apply overlays to [images](../../features/image-transformations/overlay-using-layers.md) and [videos](../../features/video-transformation/overlay.md) using the raw parameter with the concept of [layers](../../features/image-transformations/overlay-using-layers.md#layers). The raw parameter facilitates incorporating transformations directly in the URL. A layer is a distinct type of transformation that allows you to define an asset to serve as an overlay, along with its positioning and additional transformations.

#### Text as overlays

You can add any text string over a base video or image using a text layer (l-text).

For example:

```php
$imageURL = $imageKit->url(array(
    'path' => '/default-image.jpg',
    'urlEndpoint' => 'https://ik.imagekit.io/your_imagekit_id'
    
    'transformation' => [
        [
            'height' => '300',
            'width' => '400',
            'raw': "l-text,i-Imagekit,fs-50,l-end"
        ]
    ]
));
```
#### Sample Result URL
```
https://ik.imagekit.io/your_imagekit_id/tr:h-300,w-400,l-text,i-Imagekit,fs-50,l-end/default-image.jpg
```

**Output Image:**

![Overlay text over image](<../../.gitbook/assets/text-overlay-image.png>)

#### Image as overlays

You can add an image over a base video or image using an image layer (l-image).

For example:

```php
$imageURL = $imageKit->url(array(
    'path' => '/default-image.jpg',
    'urlEndpoint' => 'https://ik.imagekit.io/your_imagekit_id'
    
    'transformation' => [
        [
            'height' => '300',
            'width' => '400',
            'raw': "l-image,i-default-image.jpg,w-100,b-10_CDDC39,l-end"
        ]
    ]
));
```
#### Sample Result URL
```
https://ik.imagekit.io/your_imagekit_id/tr:h-300,w-400,l-image,i-default-image.jpg,w-100,b-10_CDDC39,l-end/default-image.jpg
```

**Output Image:**

![Overlay image over another image](<../../.gitbook/assets/image-overlay-image.png>)

#### Solid color blocks as overlays

You can add solid color blocks over a base video or image using an image layer (l-image).

For example:

```php
$imageURL = $imageKit->url(array(
    'path' => '/default-image.jpg',
    'urlEndpoint' => 'https://ik.imagekit.io/your_imagekit_id'
    
    'transformation' => [
        [
            'height' => '300',
            'width' => '400',
            'raw': "l-image,i-ik_canvas,bg-FF0000,w-300,h-100,l-end"
        ]
    ]
));
```
#### Sample Result URL
```
https://ik.imagekit.io/your_imagekit_id/tr:h-300,w-400,l-image,i-ik_canvas,bg-FF0000,w-300,h-100,l-end/default-image.jpg
```

**Output Image:**

![Overlay solid color over image](<../../.gitbook/assets/solid-color-overlay-image.png>)

### 6. Signed URL

Signed URL that expires in 300 seconds with the default URL endpoint and other query parameters.
For a detailed explanation of the Signed URL refer to this [Official Doc](https://docs.imagekit.io/features/security/signed-urls).

#### Example
```php  
$imageURL = $imageKit->url([
    "path" => "/default-image.jpg",
    "queryParameters" => 
    [
        "v" => "123"
    ],
    "transformation" => [
        [
            "height" => "300",
            "width" => "400"
        ]
    ],
    "signed" => true,
    "expireSeconds" => 300,
]);
```  
#### Response
```  
https://ik.imagekit.io/your_imagekit_id/tr:h-300,w-400/default-image.jpg?v=123&ik-t=1654183277&ik-s=f98618f264a9ccb3c017e7b7441e86d1bc9a7ebb
```  

You can manage [Security Settings](https://docs.imagekit.io/features/security#restricting-unsigned-urls) from the dashboard to prevent unsigned URLs usage. In that case, if the URL doesn't have a signature `ik-s` parameter or the signature is invalid, ImageKit will return a forbidden error instead of an actual image.

### List of supported transformations

The complete list of transformations supported and their usage in ImageKit can be found [here](https://docs.imagekit.io/features/image-transformations). The SDK gives a name to each transformation parameter, making the code simpler and readable. If a transformation is supported in ImageKit, but a name for it cannot be found in the table below, use the transformation code from ImageKit docs as the name when using it in the `url` function.

| Supported Transformation Name | Translates to parameter |
|-------------------------------|-------------------------|
| height | h |
| width | w |
| aspectRatio | ar |
| quality | q |
| crop | c |
| cropMode | cm |
| x | x |
| y | y |
| focus | fo |
| format | f |
| radius | r |
| background | bg |
| border | b |
| rotation | rt |
| blur | bl |
| named | n |
| progressive | pr |
| lossless | lo |
| trim | t |
| metadata | md |
| colorProfile | cp |
| defaultImage | di |
| dpr | dpr |
| effectSharpen | e-sharpen |
| effectUSM | e-usm |
| effectContrast | e-contrast |
| effectGray | e-grayscale |
| original | orig |
| raw | `replaced by the parameter value` |


## Server-side File Upload

The SDK provides a simple interface using the `$imageKit->uploadFile()` or `$imageKit->uploadFile()` method to upload files to the [ImageKit Media Library](https://imagekit.io/dashboard/media-library). 

- [Check all the supported file types and extensions](https://docs.imagekit.io/api-reference/upload-file-api#allowed-file-types-for-uploading).
- [Check all the supported parameters and details](https://docs.imagekit.io/api-reference/upload-file-api/server-side-file-upload).

#### Example
```php
$uploadFile = $imageKit->uploadFile([
    'file' => 'your_file',              //  required, "binary","base64" or "file url"
    'fileName' => 'your_file_name.jpg', //  required
]);
```
#### Response
```json
{
    "error": null,
    "result": {
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
    },
    "responseMetadata":{
        "headers":{
            "access-control-allow-origin": "*",
            "x-ik-requestid": "e98f2464-2a86-4934-a5ab-9a226df012c9",
            "content-type": "application/json; charset=utf-8",
            "content-length": "434",
            "etag": "W/"1b2-reNzjRCFNt45rEyD7yFY/dk+Ghg"",
            "date": "Thu, 16 Jun 2022 14:22:01 GMT",
            "x-request-id": "e98f2464-2a86-4934-a5ab-9a226df012c9"
        },
        "raw":{
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
        },
        "statusCode":200
    }
}
```
#### Optional Parameters
Please refer to [Server Side File Upload - Request Structure](https://docs.imagekit.io/api-reference/upload-file-api/server-side-file-upload#request-structure-multipart-form-data) for detailed explanation about mandatory and optional parameters.
```php
// Attempt File Uplaod
$uploadFile = $imageKit->uploadFile([
    'file' => 'your_file',                  //  required, "binary","base64" or "file url"
    'fileName' => 'your_file_name.jpg',     //  required
    // Optional Parameters
    "useUniqueFileName" => true,            // true|false
    "tags" => implode(",",["abd", "def"]),  // max: 500 chars
    "folder" => "/sample-folder",           
    "isPrivateFile" => false,               // true|false
    "customCoordinates" => implode(",", ["10", "10", "100", "100"]),    // max: 500 chars
    "responseFields" => implode(",", ["tags", "customMetadata"]),
    "extensions" => [       
        [
            "name" => "remove-bg",
            "options" => [  // refer https://docs.imagekit.io/extensions/overview
                "add_shadow" => true
            ]
        ]
    ],
    "webhookUrl" => "https://example.com/webhook",
    "overwriteFile" => true,        // in case of false useUniqueFileName should be true
    "overwriteAITags" => true,      // set to false in order to preserve overwriteAITags
    "overwriteTags" => true,
    "overwriteCustomMetadata" => true,
    // "customMetadata" => [
    //         "SKU" => "VS882HJ2JD",
    //         "price" => 599.99,
    // ]
]);
```  


## ImageKit Media API

The SDK provides a simple interface for all the following [Media APIs](https://docs.imagekit.io/api-reference/media-api) to manage your files.

### 1. List and Search Files

This API can list all the uploaded files and folders in your [ImageKit.io](https://docs.imagekit.io/api-reference/media-api) media library. 

Refer to the [List and Search File API](https://docs.imagekit.io/api-reference/media-api/list-and-search-files) for a better understanding of the **Request & Response Structure**.

#### Example
```php
$listFiles = $imageKit->listFiles();
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
In addition, you can fine-tune your query by specifying various filters by generating a query string in a Lucene-like syntax and providing this generated string as the value of the `searchQuery`.
```PHP
$listFiles = $imageKit->listFiles([
    "searchQuery" => '(size < "1mb" AND width > 500) OR (tags IN ["summer-sale","banner"])',
]);
```
Detailed documentation can be found here for [Advance Search Queries](https://docs.imagekit.io/api-reference/media-api/list-and-search-files#advanced-search-queries).

### 2. Get File Details

This API can get you all the details and attributes of the current version of the file.

Refer to the [Get File Details API](https://docs.imagekit.io/api-reference/media-api/get-file-details) for a better understanding of the **Request & Response Structure**.

#### Example
```php
$getFileDetails = $imageKit->getFileDetails('file_id');
```

### 3. Get File Version Details

This API can get you all the details and attributes for the provided version of the file.`versionID` can be found in the following APIs as `id` within the `versionInfo` parameter:
- [Server-side File Upload API](#server-side-file-upload).
- [List & Search File API](#1-list-and-search-files)
- [Get File Details API](#2-get-file-details)

Refer to the [Get File Version Details API](https://docs.imagekit.io/api-reference/media-api/get-file-version-details) for a better understanding of the **Request & Response Structure**.

#### Example
```php
$getFileVersionDetails = $imageKit->getFileVersionDetails('file_id','version_id');
```

### 4. Get File Versions

This API can get you all the versions of the file.

Refer to the [Get File Versions API](https://docs.imagekit.io/api-reference/media-api/get-file-versions) for a better understanding of the **Request & Response Structure**.

#### Example
```php
$getFileVersions = $imageKit->getFileVersions('file_id');
```

### 5. Update File Details

Update file details such as tags, customCoordinates attributes, remove existing AITags, and apply [extensions](https://docs.imagekit.io/extensions/overview) using Update File Details API. This operation can only be performed on the current version of the file.

Refer to the [Update File Details API](https://docs.imagekit.io/api-reference/media-api/update-file-details) for better understanding about the **Request & Response Structure**.

#### Example
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

### 6. Add Tags (Bulk) API

Add tags to multiple files in a single request. The method accepts an array of `fileIds` of the files and an array of `tags` that have to be added to those files.

Refer to the [Add Tags (Bulk) API](https://docs.imagekit.io/api-reference/media-api/add-tags-bulk) for a better understanding of the **Request & Response Structure**.

#### Example
```php
$fileIds = ['file_id1','file_id2'];
$tags = ['image_tag_1', 'image_tag_2'];

$bulkAddTags = $imageKit->bulkAddTags($fileIds, $tags);
```

### 7. Remove Tags (Bulk) API

Remove tags from multiple files in a single request. The method accepts an array of `fileIds` of the files and an array of `tags` that have to be removed from those files.

Refer to the [Remove Tags (Bulk) API](https://docs.imagekit.io/api-reference/media-api/remove-tags-bulk) for a better understanding of the **Request & Response Structure**.

#### Example
```php
$fileIds = ['file_id1','file_id2'];
$tags = ['image_tag_1', 'image_tag_2'];

$bulkRemoveTags = $imageKit->bulkRemoveTags($fileIds, $tags);
```

### 8. Remove AI Tags (Bulk) API

Remove AI tags from multiple files in a single request. The method accepts an array of `fileIds` of the files and an array of `AITags` that have to be removed from those files.

Refer to the [Remove AI Tags (Bulk) API](https://docs.imagekit.io/api-reference/media-api/remove-aitags-bulk) for a better understanding of the **Request & Response Structure**.

#### Example
```php
$fileIds = ['file_id1','file_id2'];
$AITags = ['image_AITag_1', 'image_AITag_2'];

$bulkRemoveTags = $imageKit->bulkRemoveTags($fileIds, $AITags);
```

### 9. Delete File API

You can programmatically delete uploaded files in the media library using delete file API.

> If a file or specific transformation has been requested in the past, then the response is cached. Deleting a file does not purge the cache. You can purge the cache using [Purge Cache API](#21-purge-cache-api).

Refer to the [Delete File API](https://docs.imagekit.io/api-reference/media-api/delete-file) for better understanding about the **Request & Response Structure**.

#### Basic Usage
```php
$fileId = 'file_id';
$deleteFile = $imageKit->deleteFile($fileId);
```

### 10. Delete File Version API

You can programmatically delete the uploaded file version in the media library using the delete file version API.

> You can delete only the non-current version of a file.

Refer to the [Delete File Version API](https://docs.imagekit.io/api-reference/media-api/delete-file-version) for a better understanding of the **Request & Response Structure**.

#### Example
```php
$fileId = 'file_id';
$versionId = 'version_id';
$deleteFileVersion = $imageKit->deleteFileVersion($fileId, $versionId);
```

### 11. Delete Files (Bulk) API

Deletes multiple files and their versions from the media library.

Refer to the [Delete Files (Bulk) API](https://docs.imagekit.io/api-reference/media-api/delete-files-bulk) for a better understanding of the **Request & Response Structure**.

#### Example
```php
$fileIds = ["5e1c13d0c55ec3437c451406", ...];
$deleteFiles = $imageKit->bulkDeleteFiles($fileIds);
```


### 12. Copy File API

This will copy a file from one folder to another.

>  If any file at the destination has the same name as the source file, then the source file and its versions (if `includeFileVersions` is set to true) will be appended to the destination file version history.

Refer to the [Copy File API](https://docs.imagekit.io/api-reference/media-api/copy-file) for a better understanding of the **Request & Response Structure**.

#### Basic Usage
```php
$sourceFilePath = '/sample-folder1/sample-file.jpg';
$destinationPath = '/sample-folder2/';
$includeFileVersions = false;

$copyFile = $imageKit->copy([
    'sourceFilePath' => $sourceFilePath,
    'destinationPath' => $destinationPath,
    'includeFileVersions' => $includeFileVersions
]);
```

### 13. Move File API

This will move a file and all its versions from one folder to another.

>  If any file at the destination has the same name as the source file, then the source file and its versions will be appended to the destination file.

Refer to the [Move File API](https://docs.imagekit.io/api-reference/media-api/move-file) for a better understanding of the **Request & Response Structure**.

#### Example
```php
$sourceFilePath = '/sample-file.jpg';
$destinationPath = '/sample-folder/';

$moveFile = $imageKit->move([
    'sourceFilePath' => $sourceFilePath,
    'destinationPath' => $destinationPath
]);
```

### 14. Rename File API

You can programmatically rename an already existing file in the media library using Rename File API. This operation would rename all file versions of the file.

>  The old URLs will stop working. The file/file version URLs cached on CDN will continue to work unless a purge is requested.

Refer to the [Rename File API](https://docs.imagekit.io/api-reference/media-api/rename-file) for a better understanding of the **Request & Response Structure**.

#### Example
```php
// Purge Cache would default to false

$filePath = '/sample-folder/sample-file.jpg';
$newFileName = 'sample-file2.jpg';
$renameFile = $imageKit->rename([
    'filePath' => $filePath,
    'newFileName' => $newFileName,
]);
```
When `purgeCache` is set to `true`, response will return `purgeRequestId`. This `purgeRequestId` can be used to get the purge request status.
```php
$filePath = '/sample-folder/sample-file.jpg';
$newFileName = 'sample-file2.jpg';
$renameFile = $imageKit->rename([
    'filePath' => $filePath,
    'newFileName' => $newFileName,
    'purgeCache' => true
]);
```

### 15. Restore File Version API

This will restore the provided file version to a different version of the file. The newly restored version of the file will be returned in the response.

Refer to the [Restore File Version API](https://docs.imagekit.io/api-reference/media-api/restore-file-version) for a better understanding of the **Request & Response Structure**.

#### Example
```php
$fileId = 'fileId';
$versionId = 'versionId';
$restoreFileVersion = $imageKit->restoreFileVersion([
    'fileId' => $fileId,
    'versionId' => $versionId,
]);
```

### 16. Create Folder API

This will create a new folder. You can specify the folder name and location of the parent folder where this new folder should be created.

Refer to the [Create Folder API](https://docs.imagekit.io/api-reference/media-api/create-folder) for a better understanding of the **Request & Response Structure**.

#### Example
```php
$folderName = 'new-folder';
$parentFolderPath = '/';
$createFolder = $imageKit->createFolder([
    'folderName' => $folderName,
    'parentFolderPath' => $parentFolderPath,
]);
```

### 17. Delete Folder API

This will delete the specified folder and all nested files, their versions & folders. This action cannot be undone.

Refer to the [Delete Folder API](https://docs.imagekit.io/api-reference/media-api/delete-folder) for a better understanding of the **Request & Response Structure**.

#### Example
```php
$folderPath = '/new-folder';
$deleteFolder = $imageKit->deleteFolder($folderPath);
```

### 18. Copy Folder API

This will copy one folder into another.

Refer to the [Copy Folder API](https://docs.imagekit.io/api-reference/media-api/copy-folder) for a better understanding of the **Request & Response Structure**.

#### Example
```php
$sourceFolderPath = '/source-folder/';
$destinationPath = '/destination-folder/';
$includeFileVersions = false;
$copyFolder = $imageKit->copyFolder([
    'sourceFolderPath' => $sourceFolderPath,
    'destinationPath' => $destinationPath,
    'includeFileVersions' => $includeFileVersions
]);
```

### 19. Move Folder API

This will move one folder into another. The selected folder, its nested folders, files, and their versions are moved in this operation.

> If any file at the destination has the same name as the source file, then the source file and its versions will be appended to the destination file version history.

Refer to the [Move Folder API](https://docs.imagekit.io/api-reference/media-api/move-folder) for a better understanding of the **Request & Response Structure**.

#### Example
```php
$sourceFolderPath = '/sample-folder/';
$destinationPath = '/destination-folder/';
$moveFolder = $imageKit->moveFolder([
    'sourceFolderPath' => $sourceFolderPath,
    'destinationPath' => $destinationPath
]);
```

### 20. Bulk Job Status API

This endpoint allows you to get the status of a bulk operation e.g. [Copy Folder API](#18-copy-folder-api) or [Move Folder API](#19-move-folder-api).

Refer to the [Bulk Job Status API](https://docs.imagekit.io/api-reference/media-api/copy-move-folder-status) for a better understanding of the **Request & Response Structure**.

#### Example
```php
$jobId = 'jobId';
$bulkJobStatus = $imageKit->getBulkJobStatus($jobId);
```

### 21. Purge Cache API

This will purge CDN and ImageKit.io's internal cache. In response, `requestId` is returned, which can be used to fetch the status of the submitted purge request with [Purge Cache Status API](#22-purge-cache-status-api).

Refer to the [Purge Cache API](https://docs.imagekit.io/api-reference/media-api/purge-cache) for a better understanding of the **Request & Response Structure**.

#### Example
```php
$image_url = 'https://ik.imagekit.io/demo/sample-folder/sample-file.jpg';
$purgeCache = $imageKit->purgeCache($image_url);
```

You can purge the cache for multiple files. Check [Purge Cache Multiple Files](https://docs.imagekit.io/api-reference/media-api/purge-cache#purge-cache-for-multiple-files).

### 22. Purge Cache Status API

Get the purge cache request status using the `requestId` returned when a purge cache request gets submitted with [Purge Cache API](#21-purge-cache-api)

Refer to the [Purge Cache Status API](https://docs.imagekit.io/api-reference/media-api/purge-cache-status) for a better understanding of the **Request & Response Structure**.

#### Example
```php
$cacheRequestId = '598821f949c0a938d57563bd';
$purgeCacheStatus = $imageKit->purgeCacheStatus($cacheRequestId);
```

### 23. Get File Metadata API (From File ID)

Get the image EXIF, pHash, and other metadata for uploaded files in the ImageKit.io media library using this API.

Refer to the [Get image metadata for uploaded media files API](https://docs.imagekit.io/api-reference/metadata-api/get-image-metadata-for-uploaded-media-files) for a better understanding of the **Request & Response Structure**.

#### Example
```php
$fileId = '598821f949c0a938d57563bd';
$getFileMetadata = $imageKit->getFileMetaData($fileId);
```

### 24. Get File Metadata API (From Remote URL)

Get image EXIF, pHash, and other metadata from ImageKit.io powered remote URL using this API.

Refer to the [Get image metadata from remote URL API](https://docs.imagekit.io/api-reference/metadata-api/get-image-metadata-from-remote-url) for a better understanding of the **Request & Response Structure**.

#### Example
```php
$image_url = 'https://ik.imagekit.io/demo/sample-folder/sample-file.jpg';
$getFileMetadataFromRemoteURL = $imageKit->getFileMetadataFromRemoteURL($image_url);
```

## Custom Metadata Fields API

Imagekit.io allows you to define a `schema` for your metadata keys, and the value filled against that key will have to adhere to those rules. You can [Create](#1-create-fields), [Read](#2-get-fields) and [Update](#3-update-fields) custom metadata rules and update your file with custom metadata value in [File update API](#5-update-file-details) or [File Upload API](#server-side-file-upload).

For a detailed explanation, refer to the [Custom Metadata Documentaion](https://docs.imagekit.io/api-reference/custom-metadata-fields-api).


### 1. Create Fields

Create a Custom Metadata Field with this API.

Refer to the [Create Custom Metadata Fields API](https://docs.imagekit.io/api-reference/custom-metadata-fields-api/create-custom-metadata-field) for a better understanding of the **Request & Response Structure**.

#### Example
```php
$body = [
    "name" => "price",              // required
    "label" => "Unit Price",        // required
    "schema" => [                   // required
        "type" => 'Number',         // required
        "minValue" => 1000,
        "maxValue" => 5000,
    ],
];

$createCustomMetadataField = $imageKit->createCustomMetadataField($body);
```

Check for the [Allowed Values In The Schema](https://docs.imagekit.io/api-reference/custom-metadata-fields-api/create-custom-metadata-field#allowed-values-in-the-schema-object).

### 2. Get Fields

Get a list of all the custom metadata fields.

Refer to the [Get Custom Metadata Fields API](https://docs.imagekit.io/api-reference/custom-metadata-fields-api/get-custom-metadata-field) for a better understanding of the **Request & Response Structure**.

#### Example
```php
$includeDeleted = false;
$getCustomMetadataField = $imageKit->getCustomMetadataField($includeDeleted);
```

### 3. Update Fields

Update an existing custom metadata field's `label` or `schema`.

Refer to the [Update Custom Metadata Fields API](https://docs.imagekit.io/api-reference/custom-metadata-fields-api/update-custom-metadata-field) for a better understanding of the **Request & Response Structure**.

#### Example
```php
$customMetadataFieldId = '598821f949c0a938d57563dd';
$body = [
    "label" => "Net Price",
    "schema" => [
        "type"=>'Number'
    ],
];

$updateCustomMetadataField = $imageKit->updateCustomMetadataField($customMetadataFieldId, $body);
```

Check for the [Allowed Values In The Schema](https://docs.imagekit.io/api-reference/custom-metadata-fields-api/create-custom-metadata-field#allowed-values-in-the-schema-object).


### 4. Delete Fields

Delete a custom metadata field.

Refer to the [Delete Custom Metadata Fields API](https://docs.imagekit.io/api-reference/custom-metadata-fields-api/delete-custom-metadata-field) for a better understanding of the **Request & Response Structure**.

#### Example
```php
$customMetadataFieldId = '598821f949c0a938d57563dd';

$deleteCustomMetadataField = $imageKit->deleteCustomMetadataField($customMetadataFieldId);
```

## Utility Functions

We have included the following commonly used utility functions in this SDK.

### Authentication parameter generation

If you want to implement client-side file upload, you will need a `token`, `expiry` timestamp, and a valid `signature` for that upload. The SDK provides a simple method that you can use in your code to generate these authentication parameters for you.

{% hint style="danger" %}
The Private API Key should never be exposed in any client-side code. Instead, you must always generate these authentication parameters on the server side.
{% endhint %}

```
$imageKit->getAuthenticationParameters($token = "", $expire = 0);
```

It will return

```JSON
{
    "token": "5d1c4a22-54f2-40bb-9e8c-99daaeeb7307",
    "expire": 1654207193,
    "signature": "a03a88b814570a3d92919c16a1b8bd4491f053c3"
}
```  

Both the `token` and `expire` parameters are optional. If not specified, the SDK internally generates a random token and a valid expiry timestamp. The value of the `token` and `expire` used to generate the signature is always returned in the response, whether it is provided as an input to this method or not.

### Distance calculation between two pHash values

Perceptual hashing allows you to construct a hash value that uniquely identifies an input image based on the contents of an image. [ImageKit.io metadata API](https://docs.imagekit.io/api-reference/metadata-api) returns the pHash value of an image in the response. You can use this value to find a duplicate (or similar) image by calculating the distance between the pHash value of the two images.

This SDK exposes the `pHashDistance` function to calculate the distance between two pHash values. It accepts two pHash hexadecimal strings and returns a numeric value indicative of the level of difference between the two images.

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