# Upload file API

ImageKit.io allows you to upload a file (image and non-image) via API. Based on your requirement, you can either:

1. [Upload file from a server](server-side-file-upload.md) using private API key-based authentication, or
2. [Upload file from client-side](client-side-file-upload.md) (in Javascript, or Android or iPhone app) using signature-based authentication.

Both methods use the same endpoint, but there is a slight change in the request bodies.

{% hint style="info" %}
**File size limit**\
The maximum upload file size is limited to 25MB.
{% endhint %}

## Allowed file types for uploading

ImageKit.io allows you to upload a file with the following mime types:

| Allowed file types | Description            |
| ------------------ | ---------------------- |
| jpg                | JPG image file format  |
| jpeg               | JPEG image file format |
| png                | PNG image files        |
| webp               | WebP image files       |
| gif                | GIF image files        |
| svg                | SVG image files        |
| pdf                | PDF documents          |
| js                 | Javascript files       |
| css                | CSS files              |
| woff2              | Font files             |
| woff               | Font files             |
| ttf                | Font files             |
| otf                | Font files             |
| eot                | Font files             |
| txt                | Text files             |
| mp4                | MP4 video files        |
| webm               | WebM video files       |
| mov                | Movie files            |
| swf                | SWF files              |
| ts                 | Video files            |
| m3u8               | Video files            |


## Bulk file upload

The upload API can upload one file at a time. To upload files in bulk, write a program to loop through files and call upload API repeatedly. Leverage [ImageKit.io server-side SDKs](../api-introduction/sdk.md#server-side-sdks) to implement bulk upload program.