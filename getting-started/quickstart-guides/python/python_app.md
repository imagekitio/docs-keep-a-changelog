---
description: >-
  Real-time image resizing, automatic optimization, and file uploading in Python
  application using ImageKit.io.
---

# Python Application

This quick start guide shows you how to integrate ImageKit into the Python application. The code samples covered here are hosted on Github -  [https://github.com/imagekit-samples/quickstart/tree/master/python](https://github.com/imagekit-samples/quickstart/tree/master/python).

This guide walks you through the following topics:

* [Setting up ImageKit Java SDK](python_app.md#setting-up-imagekit-java-sdk)
* [Upload images](python_app.md#uploading-images-in-java-app)
* [Rendering images](python_app.md#generating-url-for-rendering-images-in-java-app)
* [Applying common image manipulations](python_app.md#common-image-manipulation-in-java-app)
* [Secure signed URL generation](python_app.md#6-secure-signed-url-generation)
* [Server-side file uploading](python_app.md#server-side-file-upload)
* [ImageKit Media API](python_app.md#imagekit-media-api)

## Setting up ImageKit Java SDK

We will create a new Python application for this tutorial and work with it.

First, we will install the imagekitio dependencies in our machine by applying the following things to our application.

## Install dependencies

```shell
pip install imagekitio
```

## Quick Examples

It loads the imagekitio dependency in our application. Before the SDK can be used, let's learn about and configure the requisite authentication parameters that need to be provided to the SDK.

#### Initialize SDK

In main file of project add your public and private API keys, as well as the URL Endpoint parameters for authentication, (You can find these keys in the Developer section of your ImageKit Dashboard)

```python
#  Put essential values of keys [UrlEndpoint, PrivateKey, PublicKey]
from imagekitio import ImageKit
imagekit = ImageKit(
    private_key='your private_key',
    public_key='your public_key',
    url_endpoint = 'your url_endpoint'
)
```

The imagekitio client is configured with user-specific credentials.

* `publicKey` and `privateKey` parameters are required as these would be used for all ImageKit API, server-side upload, and generating tokens for client-side file upload. You can get these parameters from the developer section in your ImageKit dashboard - [https://imagekit.io/dashboard/developer/api-keys](https://imagekit.io/dashboard/developer/api-keys).
* `urlEndpoint` is also a required parameter. You can get the value of URL-endpoint from your ImageKit dashboard - [https://imagekit.io/dashboard/url-endpoints](https://imagekit.io/dashboard/url-endpoints).&#x20;

## **Uploading images in java app**

There are different ways to upload the file in imagekitio. Let's upload the remote file to imagekitio using the following code:

#### Example
```python
url = "https://file-examples.com/wp-content/uploads/2017/10/file_example_JPG_100kB.jpg"
upload = imagekit.upload_file(
        file=url,
        file_name="test-url.jpg",
        options={
            "response_fields": ["is_private_file", "tags"],
            "tags": ["tag1", "tag2"],
        },
    )
```

The output should like this:
```python
upload remote file =>
{
    'error': None,
    'response': {
        'file_id': 'file_id',
        'name': 'test-url_uszVMbcXH.jpg',
        'url': 'https://ik.imagekit.io/your_imagekit_id/test-url_uszVMbcXH.jpg',
        'thumbnail_url': None,
        'height': None,
        'width': None,
        'size': 1222,
        'file_path': '/test-url_uszVMbcXH.jpg',
        'tags': ['tag1', 'tag2'],
        'ai_tags': None,
        'version_info': {
            'id': 'version_id',
            'name': 'Version 1'
        },
        'is_private_file': False,
        'custom_coordinates': None,
        'custom_metadata': None,
        'embedded_metadata': None,
        'extension_status': None,
        'file_type': 'non-image',
        '_response_metadata': {
            'raw': {
                'fileId': 'file_id',
                'name': 'test-url_uszVMbcXH.jpg',
                'size': 1222,
                'versionInfo': {
                    'id': 'version_id',
                    'name': 'Version 1'
                },
                'filePath': '/test-url_uszVMbcXH.jpg',
                'url': 'https://ik.imagekit.io/your_imagekit_id/test-url_uszVMbcXH.jpg',
                'fileType': 'non-image',
                'tags': ['tag1', 'tag2'],
                'AITags': None,
                'isPrivateFile': False
            },
            'httpStatusCode': 200,
            'headers': {
                'access-control-allow-origin': '*',
                'x-ik-requestid': '1c6a5aad-5ae2-4a6f-9b9d-adba88f0de82',
                'content-type': 'application/json; charset=utf-8',
                'content-length': '331',
                'etag': 'W/"14b-YpacFYkpdLfpfZEwh38aymvxNnE"',
                'date': 'Tue, 02 Aug 2022 11:14:48 GMT',
                'x-request-id': '1c6a5aad-5ae2-4a6f-9b9d-adba88f0de82'
            }
        }
    }
}
```

Congratulation file uploaded successfully.

## **Generating URL for rendering images in python app**
Here, declare a variable to store the image URL generated by the SDK. Like this:

#### Example
```python

image_url = imagekit.url({
            "path": "/default-image.jpg"
        }
    )
```

Now, `image_url` has the URL `https://ik.imagekit.io/<your_imagekit_id>/default-image.jpg` stored in it.
This fetches the image from the URL stored in `image_url`.

open the url on browser, it should now display this default image in its full size:

![Image in its original dimensions (1000px \* 1000px)](<../../../.gitbook/assets/python-app-default-image.png>)

## Common image manipulation in java App

Let’s now learn how to manipulate images using [ImgeKit transformations](../../../features/image-transformations/).

### **Height and width manipulation**

To resize an image or video along with its height or width, we need to pass the `transformation` option in `imageKit.url()` method.

Let's resize the default image to 200px height and width:

#### Example
```python
image_url = imagekit.url(
        {
            "path": "/default-image.jpg",
            "transformation": [{"height": "200", "width": "200", "raw": "ar-4-3,q-40"}],
        }
    )
```

**Transformation URL:**

```http
https://ik.imagekit.io/fl0osl7rq9/tr:h-200,w-200,raw-ar-4-3,q-40/default-image.jpg
```

Refresh your browser with a new url to get the resized image.

![Resized image (200px \* 200px)](<../../../.gitbook/assets/python-app-resized-image.png>)


The `imageKit.url()` method accepts the following parameters.

| Option                | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |  
| :-------------------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |  
| urlEndpoint           | Optional. The base URL is to be appended before the path of the image. If not specified, the URL Endpoint specified at the time of SDK initialization is used. For example, https://ik.imagekit.io/your_imagekit_id/endpoint/                                                                                                                                                                                                                                                                                                                                                               |  
| path                  | Conditional. This is the path on which the image exists. For example, `/path/to/image.jpg`. Either the `path` or `src` parameter needs to be specified for URL generation.                                                                                                                                                                                                                                                                                                                                                                                                                |  
| src                   | Conditional. This is the complete URL of an image already mapped to ImageKit. For example, `https://ik.imagekit.io/your_imagekit_id/endpoint/path/to/image.jpg`. Either the `path` or `src` parameter needs to be specified for URL generation.                                                                                                                                                                                                                                                                                                                                           |  
| transformation        | Optional. An array of objects specifying the transformation to be applied in the URL. The transformation name and the value should be specified as a key-value pair in the object. Different steps of a [chained transformation](https://docs.imagekit.io/features/image-transformations/chained-transformations) can be specified as different objects of the array. The complete [List of supported transformations](#list-of-supported-transformations) in the SDK and some examples of using them are given later. If you use a transformation name that is not specified in the SDK, it gets applied as it is in the URL. |  
| transformationPosition | Optional. The default value is `path`, which places the transformation string as a path parameter in the URL. It can also be specified as `query`, which adds the transformation string as the query parameter `tr` in the URL. The transformation string is always added as a query parameter if you use the `src` parameter to create the URL.                                                                                                                                                                                                                                                 |  
| queryParameters       | Optional. These are the other query parameters that you want to add to the final URL. These can be any query parameters and are not necessarily related to ImageKit. Especially useful if you want to add some versioning parameters to your URLs.                                                                                                                                                                                                                                                                                                                                           |  
| signed                | Optional. Boolean. The default value is `false`. If set to `true`, the SDK generates a signed image URL adding the image signature to the image URL.                                                                                                                                                                                                                                                                                                              |  
| expireSeconds         | Optional. Integer. It is used along with the `signed` parameter. It specifies the time in seconds from now when the signed URL will expire. If specified, the URL contains the expiry timestamp in the URL, and the image signature is modified accordingly.

### Applying Chained Transformations, Common Image Manipulations & Signed URL

This section covers the basics:

* [Chained Transformations](#1-chained-transformations)
* [Image Enhancement & Color Manipulation](#2-image-enhancement-and-color-manipulation)
* [Resizing images](#3-resizing-images)
* [Quality manipulation](#4-quality-manipulation)
* [Adding overlays to images](#5-adding-overlays-to-images)
* [Signed URL](#6-secure-signed-url-generation)

The Java SDK gives a name to each transformation parameter e.g. `height` for `h` and `width` for `w` parameter. It makes your code more readable. See the [Full List of supported transformations](#list-of-supported-transformations).

👉 If the property does not match any of the available options, it is added as it is.

```python
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

For more examples, check the [Demo Application](https://github.com/imagekit-samples/quickstart/tree/master/python).

### 1. Chained Transformations

[Chained transformations](https://docs.imagekit.io/features/image-transformations/chained-transformations) provide a simple way to control the sequence in which transformations are applied.

Chained Transformations as a query parameter

Let's try it out by resizing an image, then rotating it:

#### Example
```python
image_url = imagekit.url(
        {
            "path": "/default-image.jpg",
            "transformation": [{"height": "200", "width": "200"}],
            "transformation_position": "query",
        }
    )
```

**Transformation URL:**

```http
https://ik.imagekit.io/fl0osl7rq9/default-image.jpg?tr=h-200%2Cw-200
```

**Output Image:**

![Resized and cropped (200px \* 300px)](<../../../.gitbook/assets/python-app-resized1-image.png>)

Now, rotate the image by 90 degrees.

#### Example
```python
image_url = imagekit.url(
        {
            "path": "/default-image.jpg",
            "transformation": [{"height": "300", "width": "200"}, {"rt": "90"}],
        }
    )
```

**Chained Transformation URL:**

```http
https://ik.imagekit.io/fl0osl7rq9/tr:h-300,w-200:rt-90/default-image.jpg
```

**Output Image:**

![Resized, then rotated](<../../../.gitbook/assets/python-app-resized-rotate-image.png>)

Let's flip the order of transformation and see what happens.

#### Example
```python
image_url = imagekit.url(
        {
            "path": "/default-image.jpg",
            "transformation": [{"rt": "90"}, {"height": "300", "width": "200"}],
        }
    )
```

**Chained Transformation URL:**

```http
https://ik.imagekit.io/fl0osl7rq9/tr:rt-90:h-300,w-200/default-image.jpg
```

**Output Image:**

![Rotated, then resized](<../../../.gitbook/assets/python-app-rotate-resized-image.png>)


