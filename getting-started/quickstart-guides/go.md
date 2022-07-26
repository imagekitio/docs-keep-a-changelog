---
description: >-
  Real-time image resizing, automatic optimization, and file uploading in Go
  using ImageKit.io.
---

# Go

[ImageKit's Go SDK](https://github.com/imagekit-developer/imagekit-go) provides comprehensive yet straightforward asset upload, transformation, optimization, and delivery capabilities that you can implement seamlessly in your existing Go application.

This is a quick start guide to show you how to integrate ImageKit in your Go application. The code samples covered here are hosted on Github - [https://github.com/imagekit-developer/imagekit-Go/tree/master/sample](https://github.com/imagekit-developer/imagekit-Go/tree/master/sample)[.](https://github.com/imagekit-developer/imagekit-Go/tree/master/sample)

This guide walks you through the following topics:

* [Setting up ImageKit Go SDK](Go.md#setting-up-imagekit-Go-sdk)
* [Generating url for rendering images](Go.md#generating-url-for-rendering-images)
* [Applying common image manipulations](Go.md#applying-common-image-manipulations)
* [Adding overlays to images](Go.md#adding-overlays-to-images-in-Go)
* [Server-side file uploading](Go.md#server-side-file-uploading)
* [ImageKit Media API](Go.md#imagekit-media-api)
* [Utility Functions](Go.md#utility-functions)

## Setting up ImageKit Go SDK

#### Prerequisites

To use ImageKit Go SDK, you must be using Go version 1.18 or later. 

#### Create a new Project

```bash
mkdir sample && cd sample
go mod init sample
go get github.com/imagekit-developer/imagekit-go
```

#### Initialization

```go
import (
    "github.com/imagekit-developer/imagekit-go"
)

// Using environment variables IMAGEKIT_PRIVATE_KEY, IMAGEKIT_PUBLIC_KEY and IMAGEKIT_URL_ENDPOINT
ik, err := ImageKit.New()

// Using keys in argument
ik, err := ImageKit.NewFromParams(imagekit.NewParams{
    PrivateKey: privateKey,
    PublicKey: publicKey,
    UrlEndpoint: urlEndpoint,
})
```

* `UrlEndpoint` is the required parameter. You can get the value of URL-endpoint from your ImageKit dashboard - [https://imagekit.io/dashboard#url-endpoints](https://imagekit.io/dashboard#url-endpoints).&#x20;
* `publicKey` and `privateKey` parameters are also required as these would be used for all ImageKit API, server-side upload, and generating token for client-side file upload. You can get these parameters from the developer section in your ImageKit dashboard - [https://imagekit.io/dashboard#developers](https://imagekit.io/dashboard#developers).

## Generating url for rendering images

#### URL for image with relative path&#x20;

```go
url, err := ik.Url(url.UrlParam{
    Path: "/default-image.jpeg",
})

// Returned url: https://ik.imagekit.io/default_imagekit_id/default-image.jpg
```

![](../../.gitbook/assets/default-image.jpeg)

#### URL for image with relative path and custom URL-endpoint

```go
url, err := ik.Url(url.UrlParam{
    Path: "/default-image.jpeg",
    UrlEndpoint: "https://ik.imagekit.io/test",
})

//Returned url: https://ik.imagekit.io/test/default-image.jpg
```

#### &#x20;URL for image with absolute url&#x20;

If you have an absolute image path coming from the backend API e.g. `https://www.custom-domain.com/default-image.jpg` then you can use `src` prop to load the image.

```Go
url, err := ik.Url(url.UrlParam{
    Src: "https://ik.imagekit.io/test/default-image.jpeg",
    Transformations: []ikurl.Transformation{
        {
            Height: 300,
            Width:  400,
            Rotate: 90,
        },
    },
})

//Returned Url: https://ik.imagekit.io/test/tr:h-300,w-400:rt-90/default-image.jpg
```

`UrlParam` has following options:

| Option           | Description                    |
| :----------------| :----------------------------- |
| Path             | Conditional. This is the path at which the image exists. For example, `/path/to/image.jpg`. Either the `Path` or `Src` parameter needs to be specified for URL generation. |
| Src              | Conditional. This is the complete URL of an image already mapped to ImageKit. For example, `https://ik.imagekit.io/your_imagekit_id/endpoint/path/to/image.jpg`. Either the `Path` or `Src` parameter needs to be specified for URL generation. |
| UrlEndpoint      | Optional. The base URL to be appended before the path of the image. If not specified, the URL Endpoint specified at the time of SDK initialization is used. For example, https://ik.imagekit.io/your_imagekit_id/endpoint/ |
| Transformations   | Optional. An array of objects specifying the transformation to be applied in the URL. Different steps of a [chained transformation](https://docs.imagekit.io/features/image-transformations/chained-transformations) can be specified as different objects of the array. The complete list of supported transformations in the SDK and some examples of using them are given later. 
| TransformationPosition | Optional. The default value is `Path` that places the transformation string as a path parameter in the URL. It can also be specified as `query`, which adds the transformation string as the URL's query parameter `tr`. If you use the `Src` parameter to create the URL, then the transformation string is always added as a query parameter. |
| NamedTransformation | Optional. Specifies name of a pre defined transformation. |
| QueryParameters  | Optional. These are the other query parameters that you want to add to the final URL. These can be any query parameters and not necessarily related to ImageKit. Especially useful if you want to add some versioning parameter to your URLs. |
| Signed           | Optional. Boolean. Default is `false`. If set to `true`, the SDK generates a signed image URL adding the image signature to the image URL. If you create a URL using the `Src` parameter instead of `Path`, then do correct `UrlEndpoint` for this to work. Otherwise returned URL will have the wrong signature |
| ExpireSeconds    | Optional. Integer. Meant to be used along with the `Signed` parameter to specify the time in seconds from now when the URL should expire. If specified, the URL contains the expiry timestamp in the URL, and the image signature is modified accordingly. |


## Applying common image manipulations

This section covers the basics:

* [Chained Transformations as a query parameter](#1-chained-transformations-as-a-query-parameter)
* [Image Enhancement & Color Manipulation](#2-image-enhancement-and-color-manipulation)
* [Resizing images](#3-resizing-images)
* [Quality manipulation](#4-quality-manipulation)
* [Adding overlays to images](#5-adding-overlays-to-images)
* [Signed URL](#6-signed-url)

The Go SDK supports all image transformations as specified at [API documentation](https://docs.imagekit.io/features/image-transformations). 


### 1. Chained Transformations as a query parameter

```go
import "github.com/imagekit-developer/imagekit-go/url"

url, err := ik.Url(url.UrlParam{
    Path: "/default-image.jpg",
    UrlEndpoint: "https://ik.imagekit.io/demo/",
    Transformations: []url.Transformation{
        {
            Height: 300,
            Width:  400,
        },
        {
            Rotate: 90,
        },
    },
    TransformationPosition: url.QUERY,
})

//Result url: https://ik.imagekit.io/demo/default-image.jpg?tr=h-300,w-400:rt-90

```
![](<../../.gitbook/assets/image (68).png>)

### 2. Image enhancement and color manipulation

```go  
import "github.com/imagekit-developer/imagekit-go/url"

url, err := ik.Url(url.UrlParam{
    Src: "https://ik.imagekit.io/demo/default-image.jpg",
    Transformations: []url.Transformation{
        {
            Format: url.FJpg,
            ProgressiveImage:  true,
            Sharpen: true,
            Contrast: true,
        },
    },
})
```  
//Result url: https://ik.imagekit.io/demo/tr:f-jpg,pr-true,e-sharpen,e-contrast/default-image.jpg 
```  
![](<../../.gitbook/assets/image (69).png>)

### Sharpening and contrast transformation

You can enhance images and manipulate colors like this.

```go
url, err := ik.Url(url.UrlParam{
    Path: "/default-image.jpg",
    UrlEndpoint: "https://ik.imagekit.io/your_imagekit_id/endpoint/",
    Transformations: []ikurl.Transformation{
        {
            Format: "jpg",
            ProgressiveImage: true,
            Sharpen: true,
            Contrast: true,
        },
    },
})

//Result url:  https://ik.imagekit.io/demo/tr:f-jpg,pr-true,e-sharpen,e-contrast/sample_image.jpg
```

![](<../../.gitbook/assets/image (69).png>)

### 3. Resizing images
Let's resize the image to a width of 400 and a height of 300.
Check detailed instructions on [Resize, Crop, and Other Common Transformations](https://docs.imagekit.io/features/image-transformations/resize-crop-and-other-transformations)

```go
url, err := ik.Url(url.UrlParam{
    Path: "/default-image.jpg",
    Transformations: []ikurl.Transformation{
        {
            Height: 300,
            Width: 400,
        },
    },
})

//Result url: https://ik.imagekit.io/demo/tr:w-400,h-300/default-image.jpg
```
![400x300 image](<../../.gitbook/assets/image (66).png>)

### 4. Quality manipulation
You can use the [Quality Parameter](https://docs.imagekit.io/features/image-transformations/resize-crop-and-other-transformations#quality-q) to change quality like this.

```go
url, err := ik.Url(url.UrlParam{
    Path: "/default-image.jpg",
    Transformations: []ikurl.Transformation{
        {
            Quality: 40,
        },
    },
})

//Result url: https://ik.imagekit.io/demo/tr:q-40/default-image.jpg
```
![](<../../.gitbook/assets/image (67).png>)

### 5. Adding overlays to images
ImageKit.io allows overlaying [images](https://docs.imagekit.io/features/image-transformations/overlay#image-overlay) or [text](https://docs.imagekit.io/features/image-transformations/overlay#text-overlay) over other images for watermarking or creating a dynamic banner using custom text.

```go
import (
	"log"

	"github.com/imagekit-developer/imagekit-go"
	"github.com/imagekit-developer/imagekit-go/api"
	ikurl "github.com/imagekit-developer/imagekit-go/url"
)

url, err := ik.Url(ikurl.UrlParam {
    Path: "/default-image.jpg",
    UrlEndpoint: "https://ik.imagekit.io/pshbwfiho"
    Transformations: []ikurl.Transformation{
        {
            Height: 300,
            Width:  300,
            Overlay: &ikurl.Overlay{
                Image:       "default-image.jpg",
                Width:       api.Int(100),
                X:           api.Int(0),
                ImageBorder: "10_CDDC39",
            },
        },
    },
})
//Result url: https://ik.imagekit.io/demo/tr:w-300,h-300,oi-default-image.jpg,ow-100,ox-0,oib-10_CDDC39/default-image.jpg
```
![](<../../.gitbook/assets/image (70).png>)

### 6. Signed URL

ImageKit.io PHP SDK can generate a secure [signed url](../../features/security/signed-urls.md) that allows you to protect your media assets.

For example:

```go
url, err := ik.Url(url.UrlParam{
    Path: "/default-image.jpg",
    UrlEndpoint: "https://ik.imagekit.io/your_imagekit_id/endpoint/",
    Signed: true,
    ExpireSeconds: 300,
})

// https://ik.imagekit.io/your_imagekit_id/default-image.jpg?ik-t=1632816299&ik-s=ffe7bc9445f95b4e33b4fb25ac504557a4ae03fc
```

You can manage [security settings](../../features/security/#restricting-unsigned-urls) from the dashboard to prevent unsigned URLs usage. In that case, if the URL doesn't have signature `ik-s` parameter or the signature is invalid, ImageKit will return a forbidden error instead of an actual image.

#### List of supported transformations
url package defines transformation type:
```go
// Transformation is a struct representing options for transformation parameter to Url()
type Transformation struct {
	Width            float32     // w
	Height           float32     // h
	AspectRatio      AspectRatio // ar
	CropMode         CropMode
	Focus            Focus  // fo-custom|face|auto|left|right|top|bottom|top_left.....
	X                int    // x
	Y                int    // y
	Xc               int    // xc.  Focus with center coordinate Xc and Yc
	Yc               int    // yc
	Quality          int    // q
	Format           Format // f
	Blur             int    // bl
	Dpr              any    // dpr (auto or number 0.1 to 5)
	GrayScale        bool   // e-grayscale
	DefaultImage     string // di (Replaces all forward slashes with @@)
	ProgressiveImage bool   // pr
	Lossless         bool   // lo
	TrimEdges        any    // t (true or number 1-99)
	Border           string // b ("width_hexColorCode")
	ColorProfile     bool   // cp
	ImageMetadata    bool   // md
	Rotate           any    // rt (0 , 90 , 180 , 270 , 360 or auto)
	Radius           any    // r (number or max)
	BgColor          string // bg
	Original         bool   // orig
	Attachment       bool   // ik-attachment
	Contrast         bool
	Sharpen          any // bool or number
	Overlay          *Overlay
	Raw              string
}

// Overlay represents transformation's overlay options
type Overlay struct {
	X          *int         // ox
	Y          *int         // oy
	Height     *int         // oh
	Width      *int         // ow
	Background string       // obg
	Focus      OverlayFocus // ofo
	// Text overlay options
	Text               string // ot
	TextEncoded        string
	TextWidth          *int      // otw
	TextBackground     string    // otbg
	TextPadding        string    // otp ("40" or "40_60" or "40_60_50_10")
	TextInnerAlignment Alignment // otia
	TextColor          string    // otc
	TextFont           string    // otf
	TextSize           *int      // ots
	TextTypography     string    // ott (i, b or ib)=(italic, bold, both)
	Radius             int       // or
	// Image overlay options
	Image            string      // oi (replaces all forward slashes with @@)
	ImageAspectRatio AspectRatio // oiar
	ImageBorder      string      // oib (width_colorcode: 5_red, 10_FF0000)
	ImageDPR         *float32    // oidpr (0.1 to 5)
	ImageQuality     *int        // oiq
	ImageCropping    ImageCropping
	ImageX           *int  //oix
	ImageY           *int  //oiy
	ImageXc          *int  //oixc
	ImageYc          *int  //oixy
	Trim             *bool //oit-true
}
```

[See full documentation](https://docs.imagekit.io/features/image-transformations) for transformation options.

## Server-side file uploading

The SDK provides a simple interface using the `$imageKit->upload()` method to upload files to the ImageKit Media Library. It accepts all the parameters supported by the [ImageKit Upload API](../../api-reference/upload-file-api/server-side-file-upload.md).

The `uploadFiles()` method requires at least the `file` and the `fileName` parameter to upload a file and returns a JSON response. You can pass other parameters supported by the ImageKit upload API using the same parameter name as specified in the upload API documentation. For example, to specify tags for a file at the time of upload use the `tags` parameter as specified in the [documentation here](../../api-reference/upload-file-api/server-side-file-upload.md).

```go
import "github.com/imagekit-developer/imagekit-go/uploader"

const base64Image = "data:image/gif;base64,R0lGODlhAQABAIAAAAAAAP///yH5BAEAAAAALAAAAAABAAEAAAIBRAA7"
resp, err := ik.Uploader.Upload(ctx, base64Image, uploader.UploadParam{
    FileName: "myimage.jpg"
})

```
## ImageKit Media API

The SDK provides a simple interface for all the [media APIs mentioned here](../../api-reference/media-api/) to manage your files.

### List and search files

Accepts an object specifying the parameters to be used to list and search files. All parameters specified in the [documentation here](https://docs.imagekit.io/api-reference/media-api/list-and-search-files) can be passed as is with the correct values to get the results.

```go
import (
    "github.com/imagekit-developer/imagekit-go"
    "github.com/imagekit-developer/imagekit-go/api/media"
)

resp, err := ik.Media.Files(ctx, media.FilesParam{
    Skip: 10,
    Limit: 500,
    SearchQuery: "createdAt >= \"7d\" AND size > \"2mb\"",
})
```

### Get file details

Accepts the file ID and fetches the details as per the [API documentation here](../../api-reference/media-api/get-file-details.md).

```go
resp, err := ik.Media.FileById(ctx, "file_id")
```

### Update file details

Accepts the file ID and fetches the metadata as per the [API documentation here](../../api-reference/media-api/update-file-details.md).

```go
resp, err := ik.Media.UpdateFile(ctx, fileId, media.UpdateFileParam{
    Tags: []string{"tag_1", "tag_2"},
    RemoveAITags: []string{"car", "suv"},
})
```

### Add bulk tags

Add tags to multiple files in a single request as per [API documentation here](https://docs.imagekit.io/api-reference/media-api/add-tags-bulk). The method accepts an array of `fileIds` of the files and an array of tags that have to be added to those files.

```go
resp, err := ik.Media.AddTags(ctx, media.TagsParam{
    FileIds: []string{"file_id_1", "file_id_2"},
    Tags: []string{"tag_1", "tag_2"},
})
```

### Bulk remove tags

Remove tags from multiple files in a single request as per [API documentation here](https://docs.imagekit.io/api-reference/media-api/remove-tags-bulk). The method accepts an array of `fileIds` of the files and an array of tags that have to be removed from those files.

```go
resp, err := ik.Media.RemoveTags(ctx, media.TagsParam{
    FileIds: []string{"file_id_1", "file_id_2"},
    Tags: []string{"tag_1", "tag_2"},
})
```

### Delete file

Delete a file as per the [API documentation here](https://docs.imagekit.io/api-reference/media-api/delete-file). The method accepts the file ID of the file that has to be deleted.

```go
resp, err := ik.Media.DeleteFile(ctx, "file_id")
```

### Delete files bulk

Deletes multiple files and all their transformations as per the [API documentation here](https://docs.imagekit.io/api-reference/media-api/delete-files-bulk). The method accepts the array of file IDs that have to be deleted.
```go
resp, err := ik.Media.DeleteBulkFiles(ctx, media.FileIdsParam{
    FileIds: []string{"file_id1", "file_id2"},
)
```

### Copy file

This will copy a file from one location to another as per [API documentation here](https://docs.imagekit.io/api-reference/media-api/copy-file). This method accepts the source file's path and destination folder path.

```go
resp, err := ik.Media.CopyFile(ctx, media.CopyFileParam{
    SourcePath: "/source/a.jpg",
    DestinationPath: "/target/",
    IncludeFileVersions: false,
})
```

### Move file

This will move a file from one location to another as per [API documentation here](https://docs.imagekit.io/api-reference/media-api/move-file). This method accepts the source file's path and destination folder path.

```go
resp, err := ik.Media.MoveFile(ctx, media.MoveFileParam{
    SourcePath: "/source/a.jpg",
    DestinationPath: "/target/",
})
```

### Rename file

This will rename an already existing file in the media library as per [API Documentation here](../../api-reference/media-api/rename-file.md). This method accepts the source file's path, the new name of the file, and an optional boolean parameter to purge the CDN cache after renaming.&#x20;

{% hint style="info" %}
**Limits on purge**\
****Purging has account-level limits. Please refer to [purging limits](../../api-reference/media-api/purge-cache.md#limitations) before using this parameter.
{% endhint %}

```go
resp, err := ik.Media.RenameFile(ctx, media.RenameFileParam{
    FilePath: "/path/to/file.jpg",
    NewFileName: "newname.jpg",
    PurgeCache: true,
})

```

### Create folder

This will create a new folder as per [API documentation here](https://docs.imagekit.io/api-reference/media-api/create-folder). This method accepts the folder name and parent folder path.

```go
resp, err := ik.Media.CreateFolder(ctx, media.CreateFolderParam{
   FolderName: "nature",
   ParentFolderPath: "/some/pics"
}
```

### **Delete folder**

This will delete the specified folder and all nested files & folders as per [API documentation here](https://docs.imagekit.io/api-reference/media-api/delete-folder). This method accepts the full path of the folder that is to be deleted.

```go
resp, err := ik.Media.DeleteFolder(ctx, media.DeleteFolderParam{
    FolderPath: "/some/pics/nature",
})
```

### Copy folder

This will copy one folder into another as per [API documentation here](https://docs.imagekit.io/api-reference/media-api/copy-folder). This method accepts the source folder's path and destination folder path.

```go
resp, err := ik.Media.CopyFolder(ctx, media.CopyFolderParam{
    SourceFolderPath: "source/path",
    DestinationPath: "destination/",
    IncludeFileVersions: false
})
```

### Move folder

This will move one folder into another as per [API documentation here](https://docs.imagekit.io/api-reference/media-api/move-folder). This method accepts the source folder's path and destination folder path.

```go
resp, err := ik.Media.MoveFolder(ctx, media.MoveFolderParam{
    SourceFolderPath: "source/path",
    DestinationPath: "destination/path",
})
```

### Get bulk job status

This allows us to get a bulk operation status e.g. copy or move folder as per [API documentation here](https://docs.imagekit.io/api-reference/media-api/copy-move-folder-status). This method accepts `jobId` that is returned by copy and move folder operations.

```go
resp, err := ik.BulkJobStatus(ctx, "job_id")
```

### Purge cache

Programmatically issue a cache clear request as per the [API documentation here](https://docs.imagekit.io/api-reference/media-api/purge-cache). Accepts the full URL of the file for which the cache has to be cleared.

```go
resp, err := ik.Media.PurgeCache(ctx, media.PurgeCacheParam{
    Url: "https://ik.imageki.io/your_imagekit_id/rest-of-the-file-path.jpg"
})
```

### Purge cache status

Get the purge cache request status using the request ID returned when a purge cache request gets submitted as per the [API documentation here](https://docs.imagekit.io/api-reference/media-api/purge-cache-status)

```
resp, err := ik.Media.PurgeCacheStatus(ctx, "request_id")
```

### **Get file metadata**

Accepts the file ID and fetches the metadata as per the [API documentation here](https://docs.imagekit.io/api-reference/metadata-api/get-image-metadata-for-uploaded-media-files).

```go
resp, err := ik.Metadata.FromFile(ctx, "file_id")
```

You can also** **get metadata of the image using the absolute image URL. The image URL should be powered by ImageKit and accessible via your account.

```go
resp, err := ik.Metadata.FromUrl(ctx, "http://domian/a.jpg")
```

## Utility Functions

### Authentication parameter generation

In case you are looking to implement client-side file upload, you are going to need a `token`, `expiry` timestamp and a valid `signature` for that upload. The SDK provides a simple method that you can use in your code to generate these authentication parameters for you.

{% hint style="danger" %}
The Private API Key should never be exposed in any client-side code. You must always generate these authentication parameters on the server-side.
{% endhint %}

```go
param := SignTokenParam{Token: "31c468de-520a-4dc1-8868-de1e0fb93a7b", Expires: 1655379249}

signedToken := ik.SignToken(param)
```

It will return `SignedToken` struct variable

```go
type SignedToken struct {
	Token     string
	Expires   int64
	Signature string
}
```

Both the `token` and `expire` parameters are optional. If not specified, the SDK generates a random token and also generates a valid expiry timestamp internally. The value of the `token` and `expire` used to generate the signature are always returned in the response, no matter if they are provided as an input to this method or not.
