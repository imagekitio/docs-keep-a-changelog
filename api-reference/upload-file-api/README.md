# Upload file API

ImageKit.io allows you to upload a file (image and non-image) via API.

<b>API v1</b>

Based on your requirement, you can either:

1. [Upload file from a server](server-side-file-upload.md) using private API key-based authentication, or
2. [Upload file from client-side](client-side-file-upload.md) (in Javascript, or Android or iPhone app) using signature-based authentication.

Both methods use the same endpoint, but there is a slight change in the request bodies.

<b>API v2</b>

Based on your requirement, you can either:

1. [Upload file from a server](server-side-file-upload-v2.md) using private API key-based authentication, or
2. [File upload from client-side (secure)](secure-client-side-file-upload.md) (in Javascript or any client side application) using JSON Web Token authentication.

Both methods use the same endpoint, but there is a slight change in the request bodies.

{% hint style="info" %}
**File size limit**

The maximum upload file size is limited to 25MB on the free plan. On paid plan, this limit is 300MB for video files.
{% endhint %}

## Allowed file types for uploading

ImageKit.io supports the uploading of a wide range of file types, accommodating various formats from media files to documents.

#### How file types are identified

1. **Binary files**: Binary files, such as images, videos, and executables, are identified using the 'magic number'.
2. **Text files**: Text files, such as HTML, CSS, and JavaScript, are identified using the file extension.
3. **Text files without extension**: When a text file is uploaded without an extension, it is categorized under the content-type `application/octet-stream.’ 

## Bulk file upload

The upload API can upload one file at a time. To upload files in bulk, write a program to loop through files and call upload API repeatedly. Leverage [ImageKit.io server-side SDKs](../api-introduction/sdk.md#server-side-sdks) to implement bulk upload program.

## Access control and permissions

Read how asset ownership are affected by this operation [here](../../media-library/overview/upload-files.md#access-control-and-permissions).
