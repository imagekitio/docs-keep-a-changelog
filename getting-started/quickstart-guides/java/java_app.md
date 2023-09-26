---
description: >-
  Real-time image resizing, automatic optimization, and file uploading in Java
  application using ImageKit.io.
---

# Java Application

This quick start guide shows you how to integrate ImageKit into the Java application. The code samples covered here are hosted on Github -  [https://github.com/imagekit-samples/quickstart/tree/master/java](https://github.com/imagekit-samples/quickstart/tree/master/java).

This guide walks you through the following topics:

* [Setting up ImageKit Java SDK](java_app.md#setting-up-imagekit-java-sdk)
* [Upload images](java_app.md#uploading-images-in-java-app)
* [Rendering images](java_app.md#generating-url-for-rendering-images-in-java-app)
* [Applying common image manipulations](java_app.md#common-image-manipulation-in-java-app)
* [Secure signed URL generation](java_app.md#6-secure-signed-url-generation)
* [Server-side file uploading](java_app.md#server-side-file-upload)
* [ImageKit Media API](java_app.md#imagekit-media-api)

## Setting up ImageKit Java SDK

We will create a new Java application for this tutorial and work with it.

First, we will install the imagekitio dependencies in our machine by applying the following things to our application.

## Install dependencies

- Java 1.8 or later
### Gradle users
Step 1. Add the JitPack repository to your build file
```
allprojects {
    repositories {
        ...
        maven { url 'https://jitpack.io' }
    }
}
```
Step 2. Add the dependency on project's `build.gradle`:
```
dependencies {
        implementation 'com.github.imagekit-developer:imagekit-java:2.0.0'
}
```
### Maven users
Step 1. Add the JitPack repository to your build file
```
<repositories>
    <repository>
        <id>jitpack.io</id>
        <url>https://jitpack.io</url>
    </repository>
</repositories>
```
Step 2. Add the dependency in the POM file:
```
<dependency>
    <groupId>com.github.imagekit-developer</groupId>
    <artifactId>imagekit-java</artifactId>
    <version>2.0.0</version>
</dependency>
```

## Quick Examples

It loads the imagekitio dependency in our application. Before the SDK can be used, let's learn about and configure the requisite authentication parameters that need to be provided to the SDK.

#### Initialize SDK with config.properties

Open or create the `config.properties` file and add your public and private API keys, as well as the URL Endpoint, as follows no need to use quote('or ") in values: (You can find these keys in the Developer section of your ImageKit Dashboard)

```java
//  Put essential values of keys [UrlEndpoint, PrivateKey, PublicKey]
UrlEndpoint=your_url_endpoint
PrivateKey=your_private_key
PublicKey=your_public_key
```
#### Initialize SDK via Create an ImageKit Instance

```java
ImageKit imageKit = ImageKit.getInstance();
Configuration config = new Configuration(your_public_key, your_private_key, your_url_endpoint);
imageKit.setConfig(config);
```

The imagekitio client is configured with user-specific credentials.

* `publicKey` and `privateKey` parameters are required as these would be used for all ImageKit API, server-side upload, and generating tokens for client-side file upload. You can get these parameters from the developer section in your ImageKit dashboard - [https://imagekit.io/dashboard/developer/api-keys](https://imagekit.io/dashboard/developer/api-keys).
* `urlEndpoint` is also a required parameter. You can get the value of URL-endpoint from your ImageKit dashboard - [https://imagekit.io/dashboard/url-endpoints](https://imagekit.io/dashboard/url-endpoints).&#x20;

## **Uploading images in java app**

There are different ways to upload the file in imagekitio. Let's upload the remote file to imagekitio using the following code:

#### Example
```java
FileCreateRequest fileCreateRequest =new FileCreateRequest("https://ik.imagekit.io/ikmedia/red_dress_woman.jpeg",  "women_in_red.jpg");
List<String> responseFields=new ArrayList<>();
responseFields.add("thumbnail");
responseFields.add("tags");
responseFields.add("customCoordinates");
fileCreateRequest.setResponseFields(responseFields);
List<String> tags=new ArrayList<>();
tags.add("Software");
tags.add("Developer");
tags.add("Engineer");
fileCreateRequest.setTags(tags);
Result result=ImageKit.getInstance().upload(fileCreateRequest);
```

The output should like this:
```java
upload remote file => 
{:response=>
    Result{
        isSuccessful=true,
        message='null',
        help='null',
        fileId='62ac768c7db9372ad1f07f0b',
        name='sample-image11__Unm_54pI.jpg',
        url='https://ik.imagekit.io/zv3rkhsym/sample-image11__Unm_54pI.jpg',
        thumbnail='null',
        height=0,
        width=0,
        size=169092,
        filePath='/sample-image11__Unm_54pI.jpg',
        tags='[Software, Developer, Engineer]',
        isPrivateFile=false,
        customCoordinates='null',
        fileType='non-image',
        aiTags=null,
        versionInfo={"id":"62ac768c7db9372ad1f07f0b","name":"Version 1"},
        customMetadata=null,
        embeddedMetadata=null,
        extensionStatus=null,
        type='null',
        mime='null',
        hasAlpha=null,
        createdAt=null,
        updatedAt=null
    }
}
```

Congratulation file uploaded successfully.

## **Generating URL for rendering images in java app**
Here, declare a variable to store the image URL generated by the SDK. Like this:

#### Example
```java
Map<String, Object> options=new HashMap();
options.put("path",baseFile.getFilePath());

String image_url=ImageKit.getInstance().getUrl(options);
```

Now, `image_url` has the URL `https://ik.imagekit.io/<your_imagekit_id>/default-image.jpg` stored in it.
This fetches the image from the URL stored in `image_url`.

open the url on browser, it should now display this default image in its full size:

![Image in its original dimensions (1000px \* 1000px)](<../../../.gitbook/assets/java-app-default-image.png>)

## Common image manipulation in java App

Let’s now learn how to manipulate images using [ImgeKit transformations](../../../features/image-transformations/).

### **Height and width manipulation**

To resize an image or video along with its height or width, we need to pass the `transformation` option in `ImageKit.getInstance().getUrl()` method.

Let's resize the default image to 200px height and width:

#### Example
```java
List<Map<String, String>> transformation=new ArrayList<Map<String, String>>();
Map<String, String> scale=new HashMap<>();
scale.put("height","200");
scale.put("width","200");
scale.put("raw", "ar-4-3,q-40");
transformation.add(scale);
Map<String, Object> options=new HashMap();
options.put("path", "/default-image.jpg");
options.put("transformation", transformation);

String image_url=ImageKit.getInstance().getUrl(options);
```

**Transformation URL:**

```http
https://ik.imagekit.io/zv3rkhsym/tr:w-200,h-200,ar-4-3,q-40/default-image.jpg?ik-sdk-version=java-1.0.3
```

Refresh your browser with a new url to get the resized image.

![Resized image (200px \* 200px)](<../../../.gitbook/assets/java-app-resized-image.png>)


The `ageKit.getInstance().getUrl` method accepts the following parameters.

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

```java
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

For more examples, check the [Demo Application](https://github.com/imagekit-samples/quickstart/tree/master/java).

### 1. Chained Transformations

[Chained transformations](https://docs.imagekit.io/features/image-transformations/chained-transformations) provide a simple way to control the sequence in which transformations are applied.

Chained Transformations as a query parameter

Let's try it out by resizing an image, then rotating it:

#### Example
```java
List<Map<String, String>> transformation=new ArrayList<Map<String, String>>();
Map<String, String> scale=new HashMap<>();
scale.put("height","200");
scale.put("width","200");
transformation.add(scale);
Map<String, Object> options=new HashMap();
options.put("path", "/default-image.jpg");
options.put("transformation", transformation);
options.put("transformationPosition", "query");

String image_url=ImageKit.getInstance().getUrl(options);
```

**Transformation URL:**

```http
https://ik.imagekit.io/zv3rkhsym/default-image.jpg?ik-sdk-version=java-1.0.3&tr:w-200,h-200
```

**Output Image:**

![Resized and cropped (200px \* 300px)](<../../../.gitbook/assets/java-app-resized1-image.png>)

Now, rotate the image by 90 degrees.

#### Example
```java
List<Map<String, String>> transformation=new ArrayList<Map<String, String>>();
Map<String, String> scale=new HashMap<>();
scale.put("height","300");
scale.put("width","200");
transformation.add(scale);
Map<String, String> rotate=new HashMap<>();
rotate.put("rt","90");
transformation.add(rotate);
Map<String, Object> options=new HashMap();
options.put("path", "/default-image.jpg");
options.put("transformation", transformation);

String image_url=ImageKit.getInstance().getUrl(options);
```

**Chained Transformation URL:**

```http
https://ik.imagekit.io/zv3rkhsym/tr:w-200,h-300:rt-90/default-image.jpg?ik-sdk-version=java-1.0.3
```

**Output Image:**

![Resized, then rotated](<../../../.gitbook/assets/java-app-resized-rotate-image.png>)

Let's flip the order of transformation and see what happens.

#### Example
```java
List<Map<String, String>> transformation=new ArrayList<Map<String, String>>();
Map<String, String> rotate=new HashMap<>();
rotate.put("rt","90");
transformation.add(rotate);
Map<String, String> scale=new HashMap<>();
scale.put("height","300");
scale.put("width","200");
transformation.add(scale);
Map<String, Object> options=new HashMap();
options.put("path", "/default-image.jpg");
options.put("transformation", transformation);

String image_url=ImageKit.getInstance().getUrl(options);
```

**Chained Transformation URL:**

```http
https://ik.imagekit.io/zv3rkhsym/tr:rt-90:w-200,h-300/default-image.jpg?ik-sdk-version=java-1.0.3
```

**Output Image:**

![Rotated, then resized](<../../../.gitbook/assets/java-app-rotate-resized-image.png>)


### 2. Image enhancement and color manipulation

Some transformations like [Contrast stretch](https://docs.imagekit.io/features/image-transformations/image-enhancement-and-color-manipulation#contrast-stretch-e-contrast) , [Sharpen](https://docs.imagekit.io/features/image-transformations/image-enhancement-and-color-manipulation#sharpen-e-sharpen) and [Unsharp mask](https://docs.imagekit.io/features/image-transformations/image-enhancement-and-color-manipulation#unsharp-mask-e-usm) can be added to the URL with or without any other value. To use such transforms without specifying a value, specify the value as "-" in the transformation object. Otherwise, specify the value that you want to be added to this transformation.

#### Example
```java
Map<String, String> format=new HashMap<>();
format.put("format","jpg");
format.put("progressive","true");
format.put("effectSharpen","-");
format.put("effectContrast","1");

List<Map<String, String>> transformation= new ArrayList<>();
transformation.add(format);
Map<String, Object> options=new HashMap();
options.put("path", "/default-image.jpg");
options.put("transformation", transformation);

String image_url=ImageKit.getInstance().getUrl(options);
```

**Transformation URL:**

```http
https://ik.imagekit.io/zv3rkhsym/tr:f-jpg,pr-true,e-sharpen,e-contrast-1/default-image.jpg?ik-sdk-version=java-1.0.3
```

**Output Image:**

![Overlay image over another image](<../../../.gitbook/assets/java-app-with-enhancement-image.png>)

### 3. Resizing images
Let's resize the image to a width of 200 and a height of 200.
Check detailed instructions on [Resize, Crop, and Other Common Transformations](https://docs.imagekit.io/features/image-transformations/resize-crop-and-other-transformations)

#### Example
```java
List<Map<String, String>> transformation=new ArrayList<Map<String, String>>();
Map<String, String> scale=new HashMap<>();
scale.put("height","200");
scale.put("width","200");
transformation.add(scale);
Map<String, Object> options=new HashMap();
options.put("path", "/default-image.jpg");
options.put("transformation", transformation);

String image_url=ImageKit.getInstance().getUrl(options);
```

**Transformation URL:**

```http
https://ik.imagekit.io/zv3rkhsym/tr:w-200,h-200/default-image.jpg?ik-sdk-version=java-1.0.3
```

**Output Image:**

![Overlay image over another image](<../../../.gitbook/assets/java-app-with-resizing-image.png>)

### 4. Quality manipulation
You can use the [Quality Parameter](https://docs.imagekit.io/features/image-transformations/resize-crop-and-other-transformations#quality-q) to change quality like this.

#### Example
```java
List<Map<String, String>> transformation=new ArrayList<Map<String, String>>();
Map<String, String> quality=new HashMap<>();
quality.put("quality","40");
transformation.add(quality);
Map<String, Object> options=new HashMap();
options.put("path", "/default-image.jpg");
options.put("transformation", transformation);

String image_url=ImageKit.getInstance().getUrl(options);
```

**Transformation URL:**

```http
https://ik.imagekit.io/zv3rkhsym/tr:q-40/default-image.jpg?ik-sdk-version=java-1.0.3
```

**Output Image:**

![Overlay image over another image](<../../../.gitbook/assets/java-app-with-quality-image.png>)

### 5. Adding overlays to images
ImageKit.io allows overlaying [images](https://docs.imagekit.io/features/image-transformations/overlay#image-overlay) or [text](https://docs.imagekit.io/features/image-transformations/overlay#text-overlay) over other images for watermarking or creating a dynamic banner using custom text.

#### **Text overlay**

Text overlay can be used to superimpose text on an image. For example:

#### Example
```java
List<Map<String, String>> transformation=new ArrayList<Map<String, String>>();
Map<String, String> scale=new HashMap<>();
scale.put("height","300");
scale.put("width","300");
transformation.add(scale);
Map<String, String> overlay=new HashMap<>();
overlay.put("overlayText","ImageKit");
overlay.put("overlayTextFontSize","50");
overlay.put("overlayTextColor","0651D5");
transformation.add(overlay);
Map<String, Object> options=new HashMap();
options.put("path", "/default-image.jpg");
options.put("transformation", transformation);

String image_url=ImageKit.getInstance().getUrl(options);
```

**Transformation URL:**

```http
https://ik.imagekit.io/zv3rkhsym/tr:w-300,h-300:ots-50,ot-ImageKit,otc-0651D5/default-image.jpg?ik-sdk-version=java-1.0.3
```

**Output Image:**

![Text Overlay (300px \* 300px)](<../../../.gitbook/assets/java-app-with-overlay-text.png>)

#### **Image overlay**

Image overlay can be used to superimpose an image on another image. For example, we will upload a while logo image on [this link](https://ik.imagekit.io/demo/logo-white_SJwqB4Nfe.png) into our account and use it for the overlay image.

Base Image: `default-image.jpg`

Overlay Image: `logo-white_SJwqB4Nfe.png`

#### Example
```java
List<Map<String, String>> transformation=new ArrayList<Map<String, String>>();
Map<String, String> scale=new HashMap<>();
scale.put("height","300");
scale.put("width","300");
transformation.add(scale);
Map<String, String> overlay=new HashMap<>();
overlay.put("overlayImage","logo-white_SJwqB4Nfe.png");
transformation.add(overlay);
Map<String, Object> options=new HashMap();
options.put("path", "/default-image.jpg");
options.put("transformation", transformation);

String image_url=ImageKit.getInstance().getUrl(options);
```

**Transformation URL:**

```http
https://ik.imagekit.io/zv3rkhsym/tr:w-300,h-300:oi-logo-white_SJwqB4Nfe.png/default-image.jpg?ik-sdk-version=java-1.0.3
```

**Output Image:**

![Overlay image over another image](<../../../.gitbook/assets/java-app-with-overlay-image.png>)

### 6. Secure signed URL generation

You can use the SDK to generate a signed URL of an image, that expires in a given number of seconds.

#### Example
```java
Map<String, Object> options=new HashMap();
options.put("path", "/default-image.jpg");
options.put("signed",true);
options.put("transformation", new ArrayList<Map<String, String>>());
options.put("expireSeconds",10);

String signed_image_url=ImageKit.getInstance().getUrl(options);
```

The above snippets create a signed URL with an expiry time of 10 seconds.

![Signed URL generation](<../../../.gitbook/assets/java-app-with-signed-image.png>)

### List of supported transformations

See the complete list of [image](https://docs.imagekit.io/features/image-transformations) and [video](https://docs.imagekit.io/features/video-transformation) transformations supported in ImageKit. The SDK gives a name to each transformation parameter e.g. `height` for `h` and `width` for `w` parameter. It makes your code more readable. If the property does not match any of the following supported options, it is added as it is.

If you want to generate transformations in your application and add them to the URL as it is, use the `raw` parameter.

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
| overlayX | ox |
| overlayY | oy |
| overlayFocus | ofo |
| overlayHeight | oh |
| overlayWidth | ow |
| overlayImage | oi |
| overlayImageTrim | oit |
| overlayImageAspectRatio | oiar |
| overlayImageBackground | oibg |
| overlayImageBorder | oib |
| overlayImageDPR | oidpr |
| overlayImageQuality | oiq |
| overlayImageCropping | oic |
| overlayImageFocus | oifo |
| overlayText | ot |
| overlayTextFontSize | ots |
| overlayTextFontFamily | otf |
| overlayTextColor | otc |
| overlayTextTransparency | oa |
| overlayAlpha | oa |
| overlayTextTypography | ott |
| overlayBackground | obg |
| overlayTextEncoded | ote |
| overlayTextWidth | otw |
| overlayTextBackground | otbg |
| overlayTextPadding | otp |
| overlayTextInnerAlignment | otia |
| overlayRadius | or |
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

The SDK provides a simple interface using the `$imageKit->upload()` or `$imageKit->uploadFile()` method to upload files to the [ImageKit Media Library](https://imagekit.io/dashboard/media-library). 

- [Check all the supported file types and extensions](https://docs.imagekit.io/api-reference/upload-file-api#allowed-file-types-for-uploading).
- [Check all the supported parameters and details](https://docs.imagekit.io/api-reference/upload-file-api/server-side-file-upload).

#### Example
```java
FileCreateRequest fileCreateRequest = new FileCreateRequest(
    "your_file",            //  required, "binary", "base64" or "file url"
    "sample-image11.jpg"    //  required
);
Result result = ImageKit.getInstance().upload(fileCreateRequest);
```
#### Response
```text
Result{
    isSuccessful=true,
    message='null',
    help='null',
    fileId='62b43109d23153217b8b8a36',
    name='sample_image_To_fa4v8vk7W.jpg',
    url='https://ik.imagekit.io/zv3rkhsym/sample_image_To_fa4v8vk7W.jpg',
    thumbnail='null',
    height=300,
    width=300,
    size=51085,
    filePath='/sample_image_To_fa4v8vk7W.jpg',
    tags='null',
    isPrivateFile=false,
    customCoordinates='null',
    fileType='image'
}

```
#### Optional Parameters
Please refer to [Server Side File Upload - Request Structure](https://docs.imagekit.io/api-reference/upload-file-api/server-side-file-upload#request-structure-multipart-form-data) for a detailed explanation of mandatory and optional parameters.

#### Example
```java
FileCreateRequest fileCreateRequest = new FileCreateRequest(bytes, "your_file_name.jpg");
fileCreateRequest.setUseUniqueFileName(true);
fileCreateRequest.setPrivateFile(false);
JsonObject optionsInnerObject = new JsonObject();
optionsInnerObject.addProperty("add_shadow", true);
optionsInnerObject.addProperty("bg_colour", "green");
JsonObject innerObject1 = new JsonObject();
innerObject1.addProperty("name", "remove-bg");
innerObject1.add("options", optionsInnerObject);
JsonObject innerObject2 = new JsonObject();
innerObject2.addProperty("name", "google-auto-tagging");
innerObject2.addProperty("minConfidence", 5);
innerObject2.addProperty("maxTags", 95);
JsonArray jsonArray = new JsonArray();
jsonArray.add(innerObject1);
jsonArray.add(innerObject2);
fileCreateRequest.setExtensions(jsonArray);
List<String> responseFields = new ArrayList<>();
responseFields.add("thumbnail");
responseFields.add("tags");
responseFields.add("customCoordinates");
fileCreateRequest.setResponseFields(responseFields);
fileCreateRequest.setCustomCoordinates("10,10,40,40");
fileCreateRequest.setFolder("test");
List<String> tags = new ArrayList<>();
tags.add("tags-to-add-1");
tags.add("tags-to-add-2");
fileCreateRequest.setTags(tags);
fileCreateRequest.setWebhookUrl("https://webhook.site/c78d617f-33bc-40d9-9e61-608999721e2e");
fileCreateRequest.setOverwriteFile(true);
fileCreateRequest.setOverwriteAITags(true);
fileCreateRequest.setOverwriteTags(true);
fileCreateRequest.setOverwriteCustomMetadata(true);
JsonObject jsonObjectCustomMetadata = new JsonObject();
jsonObjectCustomMetadata.addProperty("test10", 10);
fileCreateRequest.setCustomMetadata(jsonObjectCustomMetadata);
Result result = ImageKit.getInstance().upload(fileCreateRequest);
```

## ImageKit Media API

The SDK provides a simple interface for all the following [Media APIs](https://docs.imagekit.io/api-reference/media-api) to manage your files.

### 1. List and Search Files

This API can list all the uploaded files and folders in your [ImageKit.io](https://docs.imagekit.io/api-reference/media-api) media library. 

Refer to the [List and Search File API](https://docs.imagekit.io/api-reference/media-api/list-and-search-files) for a better understanding of the **Request & Response Structure**.

#### Example
```java
GetFileListRequest getFileListRequest = new GetFileListRequest();
ResultList resultList = ImageKit.getInstance().getFileList(getFileListRequest);
```
#### Applying Filters
Filter out the files with an object specifying the parameters. 
```java
String[] tags = new String[3];
tags[0] = "tag-1";
tags[1] = "tag-2";
tags[2] = "tag-3";
GetFileListRequest getFileListRequest = new GetFileListRequest();
getFileListRequest.setType("file");
getFileListRequest.setSort("ASC_CREATED");
getFileListRequest.setPath("/");
getFileListRequest.setSearchQuery("createdAt >= '2d' OR size < '2mb' OR format='png'");
getFileListRequest.setFileType("all");
getFileListRequest.setLimit("4");
getFileListRequest.setSkip("1");
getFileListRequest.setTags(tags);
ResultList resultList = ImageKit.getInstance().getFileList(getFileListRequest);
```

#### Advance Search
In addition, you can fine-tune your query by specifying various filters by generating a query string in a Lucene-like syntax and providing this generated string as the value of the `searchQuery`.
```java
Map<String, String> options = new HashMap<>();
options.put("searchQuery", "(size < \"1mb\" AND width < 500) OR (tags IN [\"summer-sale\",\"banner\"])");       
```
Detailed documentation can be found here for [Advance Search Queries](https://docs.imagekit.io/api-reference/media-api/list-and-search-files#advanced-search-queries).

### 2. Get File Details

This API can get you all the details and attributes of the current version of the file.

Refer to the [Get File Details API](https://docs.imagekit.io/api-reference/media-api/get-file-details) for a better understanding of the **Request & Response Structure**.

#### Example
```java
Result result = ImageKit.getInstance().getFileDetail("file_id");
```

### 3. Get File Version Details

This API can get you all the details and attributes for the provided version of the file.`versionID` can be found in the following APIs as `id` within the `versionInfo` parameter:
- [Server-side File Upload API](#server-side-file-upload).
- [List & Search File API](#1-list-and-search-files)
- [Get File Details API](#2-get-file-details)

Refer to the [Get File Version Details API](https://docs.imagekit.io/api-reference/media-api/get-file-version-details) for a better understanding of the **Request & Response Structure**.

#### Example
```java

ResultFileVersionDetails resultFileVersionDetails = ImageKit.getInstance().getFileVersionDetails('file_id','version_id');
```

### 4. Get File Versions

This API can get you all the versions of the file.

Refer to the [Get File Versions API](https://docs.imagekit.io/api-reference/media-api/get-file-versions) for a better understanding of the **Request & Response Structure**.

#### Example
```java
ResultFileVersions resultFileVersions = ImageKit.getInstance().getFileVersions("file_id");
```

### 5. Update File Details

Update file details such as tags, customCoordinates attributes, remove existing AITags, and apply [extensions](https://docs.imagekit.io/extensions/overview) using Update File Details API. This operation can only be performed on the current version of the file.

Refer to the [Update File Details API](https://docs.imagekit.io/api-reference/media-api/update-file-details) for better understanding about the **Request & Response Structure**.

#### Example
```java
List<String> tags = new ArrayList<>();
tags.add("tag-1");
tags.add("tag-2");

List<String> aiTags = new ArrayList<>();
aiTags.add("ai-tag-1");
aiTags.add("ai-tag-2");
FileUpdateRequest fileUpdateRequest = new FileUpdateRequest(fileId);
fileUpdateRequest.setTags(tags);
fileUpdateRequest.setRemoveAITags(aiTags);
fileUpdateRequest.setWebhookUrl("url");

JsonObject optionsInnerObject = new JsonObject();
optionsInnerObject.addProperty("add_shadow", true);
optionsInnerObject.addProperty("bg_colour", "green");
JsonObject innerObject1 = new JsonObject();
innerObject1.addProperty("name", "remove-bg");
innerObject1.add("options", optionsInnerObject);
JsonObject innerObject2 = new JsonObject();
innerObject2.addProperty("name", "google-auto-tagging");
innerObject2.addProperty("minConfidence", 5);
innerObject2.addProperty("maxTags", 95);
JsonArray jsonArray = new JsonArray();
jsonArray.add(innerObject1);
jsonArray.add(innerObject2);
fileUpdateRequest.setExtensions(jsonArray);
fileUpdateRequest.setCustomCoordinates("10,10,40,40");
JsonObject jsonObjectCustomMetadata = new JsonObject();
jsonObjectCustomMetadata.addProperty("test10", 11);
fileUpdateRequest.setCustomMetadata(jsonObjectCustomMetadata);
Result result = ImageKit.getInstance().updateFileDetail(fileUpdateRequest);
```

### 6. Add Tags (Bulk) API

Add tags to multiple files in a single request. The method accepts an object which contains an array of `fileIds` of the files and an array of `tags` that have to be added to those files.

Refer to the [Add Tags (Bulk) API](https://docs.imagekit.io/api-reference/media-api/add-tags-bulk) for a better understanding of the **Request & Response Structure**.

#### Example
```java
List<String> fileIds = new ArrayList<>();
fileIds.add("file_id_1");
fileIds.add("file_id_2");
List<String> tags = new ArrayList<>();
tags.add("tag-to-add-1");
tags.add("tag-to-add-2");
TagsRequest tagsRequest = new TagsRequest(fileIds, tags);
tagsRequest.setFileIds(fileIds);
tagsRequest.setTags(tags);
ResultTags resultTags = ImageKit.getInstance().addTags(tagsRequest);
```

### 7. Remove Tags (Bulk) API

Remove tags from multiple files in a single request. The method accepts an object which contains an array of `fileIds` of the files and an array of `tags` that have to be removed from those files.

Refer to the [Remove Tags (Bulk) API](https://docs.imagekit.io/api-reference/media-api/remove-tags-bulk) for a better understanding of the **Request & Response Structure**.

#### Example
```java
List<String> fileIds = new ArrayList<>();
fileIds.add("file_id_1");
fileIds.add("file_id_2");
List<String> tags = new ArrayList<>();
tags.add("tag-to-remove-1");
tags.add("tag-to-remove-2");
TagsRequest tagsRequest = new TagsRequest(fileIds, tags);
ResultTags resultTags = ImageKit.getInstance().removeTags(tagsRequest);
```

### 8. Remove AI Tags (Bulk) API

Remove AI tags from multiple files in a single request. The method accepts an object which contains an array of `fileIds` of the files and an array of `AITags` that have to be removed from those files.

Refer to the [Remove AI Tags (Bulk) API](https://docs.imagekit.io/api-reference/media-api/remove-aitags-bulk) for a better understanding of the **Request & Response Structure**.

#### Example
```java
List<String> fileIds = new ArrayList<>();
fileIds.add("file_id_1");
fileIds.add("file_id_2");
List<String> aiTags = new ArrayList<>();
aiTags.add("ai-tag-to-remove-1");
aiTags.add("ai-tag-to-remove-2");
AITagsRequest aiTagsRequest = new AITagsRequest();
aiTagsRequest.setFileIds(fileIds);
aiTagsRequest.setAITags(aiTags);
ResultTags resultTags = ImageKit.getInstance().removeAITags(aiTagsRequest);
```

### 9. Delete File API

You can programmatically delete uploaded files in the media library using delete file API.

> If a file or specific transformation has been requested in the past, then the response is cached. Deleting a file does not purge the cache. You can purge the cache using [Purge Cache API](#20-purge-cache-api).

Refer to the [Delete File API](https://docs.imagekit.io/api-reference/media-api/delete-file) for better understanding about the **Request & Response Structure**.

#### Basic Usage
```java
Result result = ImageKit.getInstance().deleteFile("file_id");
```

### 10. Delete File Version API

You can programmatically delete the uploaded file version in the media library using the delete file version API.

> You can delete only the non-current version of a file.

Refer to the [Delete File Version API](https://docs.imagekit.io/api-reference/media-api/delete-file-version) for a better understanding of the **Request & Response Structure**.

#### Example
```java
DeleteFileVersionRequest deleteFileVersionRequest = new DeleteFileVersionRequest();
deleteFileVersionRequest.setFileId("file_id");
deleteFileVersionRequest.setVersionId("version_id");
ResultNoContent resultNoContent = ImageKit.getInstance().deleteFileVersion(deleteFileVersionRequest);
```

### 11. Delete Files (Bulk) API

Deletes multiple files and their versions from the media library.

Refer to the [Delete Files (Bulk) API](https://docs.imagekit.io/api-reference/media-api/delete-files-bulk) for a better understanding of the **Request & Response Structure**.

#### Example
```java
List<String> fileIds = new ArrayList<>();
fileIds.add("file-id-1");
fileIds.add("file-id-2");
ResultFileDelete result = ImageKit.getInstance().bulkDeleteFiles(fileIds);
```

### 12. Copy File API

This will copy a file from one folder to another.

>  If any file at the destination has the same name as the source file, then the source file and its versions (if `includeFileVersions` is set to true) will be appended to the destination file version history.

Refer to the [Copy File API](https://docs.imagekit.io/api-reference/media-api/copy-file) for a better understanding of the **Request & Response Structure**.

#### Basic Usage
```java
CopyFileRequest copyFileRequest = new CopyFileRequest();
copyFileRequest.setSourceFilePath("/your-file-name.jpg");
copyFileRequest.setDestinationPath("/test/");
copyFileRequest.setIncludeFileVersions(true);
ResultNoContent resultNoContent = ImageKit.getInstance().copyFile(copyFileRequest);
```

### 13. Move File API

This will move a file and all its versions from one folder to another.

>  If any file at the destination has the same name as the source file, then the source file and its versions will be appended to the destination file.

Refer to the [Move File API](https://docs.imagekit.io/api-reference/media-api/move-file) for a better understanding of the **Request & Response Structure**.

#### Example
```java
MoveFileRequest moveFileRequest = new MoveFileRequest();
moveFileRequest.setSourceFilePath("/your-file-name.jpg");
moveFileRequest.setDestinationPath("/sample-folder/");
ResultNoContent resultNoContent = ImageKit.getInstance().moveFile(moveFileRequest);
```

### 14. Rename File API

You can programmatically rename an already existing file in the media library using Rename File API. This operation would rename all file versions of the file.

>  The old URLs will stop working. The file/file version URLs cached on CDN will continue to work unless a purge is requested.

Refer to the [Rename File API](https://docs.imagekit.io/api-reference/media-api/rename-file) for a better understanding of the **Request & Response Structure**.

#### Example
```java
// Purge Cache would default to false

RenameFileRequest renameFileRequest = new RenameFileRequest();
renameFileRequest.setFilePath("/your-file-name.jpg");
renameFileRequest.setNewFileName("new-file-name.jpg");
ResultRenameFile resultRenameFile = ImageKit.getInstance().renameFile(renameFileRequest);
```
When `purgeCache` is set to `true`, response will return `purgeRequestId`. This `purgeRequestId` can be used to get the purge request status.
"`java
RenameFileRequest renameFileRequest = new RenameFileRequest();
renameFileRequest.setFilePath("/your-file-name.jpg");
renameFileRequest.setNewFileName("new-file-name.jpg");
renameFileRequest.setPurgeCache(true);
ResultRenameFile resultRenameFile = ImageKit.getInstance().renameFile(renameFileRequest);
```

### 15. Restore file Version API
This will restore file version to a different version of a file.
Refer to the [Restore file Version API](https://docs.imagekit.io/api-reference/media-api/restore-file-version) for a better understanding of the **Request & Response Structure**.
#### Example
"`java
Result result = ImageKit.getInstance().restoreFileVersion("fileId", "versionId");
```

### 16. Create Folder API

This will create a new folder. You can specify the folder name and location of the parent folder where this new folder should be created.

Refer to the [Create Folder API](https://docs.imagekit.io/api-reference/media-api/create-folder) for a better understanding of the **Request & Response Structure**.

#### Example
```java
CreateFolderRequest createFolderRequest = new CreateFolderRequest();
createFolderRequest.setFolderName("new-folder");
createFolderRequest.setParentFolderPath("/");
ResultEmptyBlock resultEmptyBlock = ImageKit.getInstance().createFolder(createFolderRequest);
```

### 17. Delete Folder API

This will delete the specified folder and all nested files, their versions & folders. This action cannot be undone.

Refer to the [Delete Folder API](https://docs.imagekit.io/api-reference/media-api/delete-folder) for a better understanding of the **Request & Response Structure**.

#### Example
```java
String folderPath = "/new-folder";
DeleteFolderRequest deleteFolderRequest = new DeleteFolderRequest();
deleteFolderRequest.setFolderPath(folderPath);
ResultNoContent resultNoContent = ImageKit.getInstance().deleteFolder(deleteFolderRequest);
```

### 18. Copy Folder API

This will copy one folder into another.
>  If any folder at the destination has the same name as the source folder, then the source folder and its versions (if `includeFileVersions` is set to true) will be appended to the destination folder version history.

Refer to the [Copy Folder API](https://docs.imagekit.io/api-reference/media-api/copy-folder) for a better understanding of the **Request & Response Structure**.

#### Example
```java
CopyFolderRequest copyFolderRequest = new CopyFolderRequest();
copyFolderRequest.setSourceFolderPath("/source-folder");
copyFolderRequest.setDestinationPath("/destination-folder");
copyFolderRequest.setIncludeFileVersions(true);
ResultOfFolderActions resultOfFolderActions = ImageKit.getInstance().copyFolder(copyFolderRequest);
```

### 19. Move Folder API

This will move one folder into another. The selected folder, its nested folders, files, and their versions are moved in this operation.

> If any file at the destination has the same name as the source file, then the source file and its versions will be appended to the destination file version history.

Refer to the [Move Folder API](https://docs.imagekit.io/api-reference/media-api/move-folder) for a better understanding of the **Request & Response Structure**.

#### Example
```java
MoveFolderRequest moveFolderRequest = new MoveFolderRequest();
moveFolderRequest.setSourceFolderPath("/source-folder");
moveFolderRequest.setDestinationPath("/destination-folder");
ResultOfFolderActions resultOfFolderActions = ImageKit.getInstance().moveFolder(moveFolderRequest);
```

### 20. Bulk Job Status API

This endpoint allows you to get the status of a bulk operation e.g. [Copy Folder API](#17-copy-folder-api) or [Move Folder API](#18-move-folder-api).

Refer to the [Bulk Job Status API](https://docs.imagekit.io/api-reference/media-api/copy-move-folder-status) for a better understanding of the **Request & Response Structure**.

#### Example
```java
String jobId = "job_id";
ResultBulkJobStatus resultBulkJobStatus = ImageKit.getInstance().getBulkJobStatus(jobId);
```

### 21. Purge Cache API

This will purge CDN and ImageKit.io's internal cache. In response `requestId` is returned which can be used to fetch the status of the submitted purge request with [Purge Cache Status API](#21-purge-cache-status-api).

Refer to the [Purge Cache API](https://docs.imagekit.io/api-reference/media-api/purge-cache) for a better understanding of the **Request & Response Structure**.

#### Example
```java
ResultCache result = ImageKit.getInstance().purgeCache("file-path");
```

### 22. Purge Cache Status API

Get the purge cache request status using the `requestId` returned when a purge cache request gets submitted with [Purge Cache API](#20-purge-cache-api)

Refer to the [Purge Cache Status API](https://docs.imagekit.io/api-reference/media-api/purge-cache-status) for a better understanding of the **Request & Response Structure**.

#### Example
```java
ResultCacheStatus result = ImageKit.getInstance().getPurgeCacheStatus("request-id");
```

### 23. Get File Metadata API (From File ID)

Get the image EXIF, pHash, and other metadata for uploaded files in the ImageKit.io media library using this API.

Refer to the [Get image metadata for uploaded media files API](https://docs.imagekit.io/api-reference/metadata-api/get-image-metadata-for-uploaded-media-files) for a better understanding of the **Request & Response Structure**.

#### Example
```java
ResultMetaData result = ImageKit.getInstance().getFileMetadata("file-id");
```

### 24. Get File Metadata API (From Remote URL)

Get image EXIF, pHash, and other metadata from ImageKit.io powered remote URL using this API.

Refer to the [Get image metadata from remote URL API](https://docs.imagekit.io/api-reference/metadata-api/get-image-metadata-from-remote-url) for a better understanding of the **Request & Response Structure**.

#### Example
```java
ResultMetaData result = ImageKit.getInstance().getRemoteFileMetadata("image_url");
```

## Custom Metadata Fields API

Imagekit.io allows you to define a `schema` for your metadata keys and the value filled against that key will have to adhere to those rules. You can [Create](#1-create-fields), [Read](#2-get-fields) [Update](#3-update-fields) and [Delete](#4-delete-fields) custom metadata rules and update your file with custom metadata value in [File update API](#5-update-file-details) or [File Upload API](#server-side-file-upload).

For a detailed explanation refer to the [Custom Metadata Documentaion](https://docs.imagekit.io/api-reference/custom-metadata-fields-api).


### 1. Create Fields

Create a Custom Metadata Field with this API.

Refer to the [Create Custom Metadata Fields API](https://docs.imagekit.io/api-reference/custom-metadata-fields-api/create-custom-metadata-field) for a better understanding of the **Request & Response Structure**.

#### Example
```java
CustomMetaDataFieldSchemaObject customMetaDataFieldSchemaObject = new CustomMetaDataFieldSchemaObject();
customMetaDataFieldSchemaObject.setType("Number");                              // required
customMetaDataFieldSchemaObject.setMinValue(10);
customMetaDataFieldSchemaObject.setMaxValue(100);

CustomMetaDataFieldCreateRequest customMetaDataFieldCreateRequest = new CustomMetaDataFieldCreateRequest();
customMetaDataFieldCreateRequest.setName("field-name");                         // required
customMetaDataFieldCreateRequest.setLabel("field-label");                       // required
customMetaDataFieldCreateRequest.setSchema(customMetaDataFieldSchemaObject);    // required

ResultCustomMetaDataField resultCustomMetaDataField = ImageKit.getInstance()
.createCustomMetaDataFields(customMetaDataFieldCreateRequest);
```

#### MultiSelect type Exmample:

```java
List<Object> objectList = new ArrayList<>();
objectList.add("small");
objectList.add(30);
objectList.add(40);
objectList.add(true);

List<Object> defaultValueObject = new ArrayList<>();
defaultValueObject.add("small");
defaultValueObject.add(30);
defaultValueObject.add(true);
CustomMetaDataFieldSchemaObject customMetaDataFieldSchemaObject = new CustomMetaDataFieldSchemaObject();
customMetaDataFieldSchemaObject.setType("MultiSelect");
customMetaDataFieldSchemaObject.setValueRequired(true);                 // optional
customMetaDataFieldSchemaObject.setDefaultValue(defaultValueObject);    // required if isValueRequired set to true
customMetaDataFieldSchemaObject.setSelectOptions(objectList);
CustomMetaDataFieldCreateRequest customMetaDataFieldCreateRequest = new CustomMetaDataFieldCreateRequest();
customMetaDataFieldCreateRequest.setName("Name-MultiSelect");
customMetaDataFieldCreateRequest.setLabel("Label-MultiSelect");
customMetaDataFieldCreateRequest.setSchema(customMetaDataFieldSchemaObject);

ResultCustomMetaDataField resultCustomMetaDataField = ImageKit.getInstance()
      .createCustomMetaDataFields(customMetaDataFieldCreateRequest);
```

Check for the [Allowed Values In The Schema](https://docs.imagekit.io/api-reference/custom-metadata-fields-api/create-custom-metadata-field#allowed-values-in-the-schema-object).

### 2. Get Fields

Get a list of all the custom metadata fields. if includeDeleted is passed and set to `true`, the list will include deleted fields also.

Refer to the [Get Custom Metadata Fields API](https://docs.imagekit.io/api-reference/custom-metadata-fields-api/get-custom-metadata-field) for a better understanding of the **Request & Response Structure**.

#### Example
```java
boolean includeDeleted = false;
ResultCustomMetaDataFieldList resultCustomMetaDataFieldList = ImageKit.getInstance()
.getCustomMetaDataFields(includeDeleted);
```

### 3. Update Fields

Update the `label` or `schema` of an existing custom metadata field.

Refer to the [Update Custom Metadata Fields API](https://docs.imagekit.io/api-reference/custom-metadata-fields-api/update-custom-metadata-field) for a better understanding of the **Request & Response Structure**.

#### Example
```java
CustomMetaDataFieldSchemaObject schemaObject = new CustomMetaDataFieldSchemaObject();
schemaObject.setMinValue(1);
schemaObject.setMaxValue(200);

CustomMetaDataFieldUpdateRequest customMetaDataFieldUpdateRequest = new CustomMetaDataFieldUpdateRequest();
customMetaDataFieldUpdateRequest.setId("field-id");
customMetaDataFieldUpdateRequest.setLabel("field-label");
customMetaDataFieldUpdateRequest.setSchema(schemaObject);
ResultCustomMetaDataField resultCustomMetaDataField = ImageKit.getInstance()
.updateCustomMetaDataFields(customMetaDataFieldUpdateRequest);
```

Check for the [Allowed Values In The Schema](https://docs.imagekit.io/api-reference/custom-metadata-fields-api/create-custom-metadata-field#allowed-values-in-the-schema-object).


### 4. Delete Fields

Delete a custom metadata field.

Refer to the [Delete Custom Metadata Fields API](https://docs.imagekit.io/api-reference/custom-metadata-fields-api/delete-custom-metadata-field) for a better understanding of the **Request & Response Structure**.

#### Example
```java
ResultNoContent resultNoContent = ImageKit.getInstance().deleteCustomMetaDataField("field-id");
```

## **What's next**

The possibilities for image manipulation and optimization with ImageKit are endless. Learn more about it here:&#x20;

* [Image Transformations](https://docs.imagekit.io/features/image-transformations)
* [Image optimization](https://docs.imagekit.io/features/image-optimization)
* [Media Library](https://docs.imagekit.io/media-library/overview)
* [Performance monitoring](../../../features/performance-monitoring.md)
