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
**File size limit**\
The maximum upload file size is limited to 25MB on the free plan. On paid plan, this limit is 300MB for video files.
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
| heif               | HEIF image files       |
| heic               | HEIC image files       |
| bmp                | BMP image files        |
| flif               | FLIF image files       |
| arw                | ARW image files        |
| tif                | TIF files              |
| tiff               | TIFF files             |
| pdf                | PDF documents          |
| js                 | Javascript files       |
| mjs                | MJS javascript files   |
| css                | CSS files              |
| php                | PHP files              |
| py                 | PY files               |
| yml                | YML files              |
| yaml               | YAML files             |
| woff2              | Font files             |
| woff               | Font files             |
| ttf                | Font files             |
| otf                | Font files             |
| eot                | Font files             |
| ps                 | PostScript files       |
| txt                | Text files             |
| mp4                | MP4 video files        |
| webm               | WebM video files       |
| mov                | Movie files            |
| swf                | SWF files              |
| ts                 | Video files            |
| m3u8               | Video playlist files   |
| json               | JSON files             |
| xml                | XML files              |
| rss                | RSS files              |
| html               | HTML files             |
| mp3                | MP3 audio files        |
| m4a                | M4A audio files        |
| wav                | WAV audio files        |
| doc                | Microsoft doc files    |
| ppt                | Microsoft ppt files    |
| xls                | Microsoft xls files    |
| docx               | Microsoft docx files   |
| pptx               | Microsoft pptx files   |
| xlsx               | Microsoft xlsx files   |
| eps                | EPS vector files       |
| eps3               | EPS3 vector files      |
| indd               | INDD files             |
| ai                 | AI vector files        |
| bw                 | BW graphic files       |  
| psd                | PSD files              |
| rtf                | RTF document files     |
| EPT                | EPT files              |
| glb                | GLB 3D files           |
| dxf                | DXF 3D files           |
| 3ds                | 3DS 3D files           |
| obj                | OBJ 3D files           |
| stl                | STL 3D files           |
| fbx                | FBX 3D files           |
| ply                | PLY 3D files           |
| gltf               | GLTF 3D files          |
| djvu               | DJVU files             |
| dng                | DNG files              |
| appimage           | APPIMAGE files         |


## Bulk file upload

The upload API can upload one file at a time. To upload files in bulk, write a program to loop through files and call upload API repeatedly. Leverage [ImageKit.io server-side SDKs](../api-introduction/sdk.md#server-side-sdks) to implement bulk upload program.

## Access control and permissions

Read how asset ownership are affected by this operation [here](../../media-library/overview/upload-files.md#access-control-and-permissions).
