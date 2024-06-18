---
description: >-
  Real-time image resizing, automatic optimization, and file uploading in
  Android using ImageKit.io.
---

# Android

This is a quick start guide to show you how to integrate ImageKit in an Android application.

## **Clone and run the tutorial app**

For this tutorial, we have used our sample Android app as shown below. If you have your own existing Android app, you can integrate the ImageKit Android SDK to it, the instructions for which are provided in the subsequent sections.

You can clone the sample app repository with the following command:

```bash
git clone https://github.com/imagekit-samples/quickstart.git
```

After cloning is completed, open the `android` folder within the cloned repository in Android Studio. Run Gradle sync to install the dependencies specified for the sample Android app. After the sync is done successfully, you can run the app by selecting your target device/emulator and clicking on the Run button in Studio.

You should see the following screen. This means the sample app has been set up correctly.

![](<../../.gitbook/assets/Screenshot 2023-09-11 at 2.05.12 PM.png>)

## Setup ImageKit Android SDK

You can integrate and use the ImageKit Android SDK in your own app by following these steps in your app project:

#### Installing the SDK

To install the SDK, add the following to the root-level `build.gradle`:
```gradle
allprojects {
    repositories {
       ...
        maven { url "https://jitpack.io" }
    }
}
```

And in the module-level `build.gradle`, add:
```gradle 
implementation 'com.github.imagekit-developer.imagekit-android:imagekit-android:<VERSION>'
```

#### Initializing the SDK

Initialize our SDK preferably in the `Application` class or any `Activity` as needed, with the following code:

```kotlin
// In Kotlin
import com.imagekit.android.ImageKit

ImageKit.init(
    context = applicationContext,
    publicKey = "your_public_api_key",
    urlEndpoint = "https://ik.imagekit.io/your_imagekit_id",
    transformationPosition = TransformationPosition.PATH,
    defaultUploadPolicy = UploadPolicy.Builder()
        .requireNetworkType(UploadPolicy.NetworkType.ANY)
        .setMaxRetries(3)
        .build()
)
```

```java
// In Java
import com.imagekit.android.ImageKit;

ImageKit.Companion.init(
    getApplicationContext(),
    "your_public_api_key",
    "https://ik.imagekit.io/your_imagekit_id",
    TransformationPosition.PATH,
    UploadPolicy.Builder()
        .requireNetworkType(UploadPolicy.NetworkType.ANY)
        .setMaxRetries(3)
        .build()
);
```

* `urlEndpoint` is the required parameter. You can get the value of the URL-endpoint from your ImageKit dashboard - https://imagekit.io/dashboard#url-endpoints.

* `transformationPosition` is optional. The default value for this parameter is `TransformationPosition.PATH`. Acceptable values are `TransformationPosition.PATH` & `TransformationPosition.QUERY`.

* `defaultUploadPolicy` is optional and only needed if you want to use the SDK for client-side file upload. This sets the default constraints for all the upload requests.

## Rendering Images in Android application

Image URL can be created from an image path or using the absolute image URL.

To render an image using an absolute URL (full image URL), we can instantiate the `ImageKitUrlConstructor`.

```kotlin
// https://ik.imagekit.io/your_imagekit_id/medium_cafe_B1iTdD0C.jpg
ImageKit.getInstance()
    .url(
        src = "https://ik.imagekit.io/your_imagekit_id/medium_cafe_B1iTdD0C.jpg",
        transformationPosition = TransformationPosition.PATH
    )
    .create()
```

To create a URL from the image path, we can instantiate the `ImageKitUrlConstructor` which takes the URL endpoint, image path, and transformation position as parameters to create the transformed url.

```kotlin
// https://ik.imagekit.io/your_imagekit_id/default-image.jpg
ImageKit.getInstance()
    .url(
        path = "default-image.jpg",
        transformationPosition = TransformationPosition.QUERY
    )
    .create()
```

{% hint style="info" %}
The transformation position (path or query) is only valid when creating a URL from the image path. Transformations are always added as query parameters if the URL is created from an absolute image path using **src**.
{% endhint %}

The Android SDK provides a [function](https://github.com/imagekit-developer/imagekit-Android#list-of-supported-transformations) for each transformation parameter, making the code simpler and readable. To add transformations, the functions can be chained with the `ImagekitUrlConstructor` instance.

```kotlin
val urlConstructor = ImageKit.getInstance()
    .url(
        path = "default-image.jpg",
        transformationPosition = TransformationPosition.QUERY
    )
    .height(400)
    .aspectRatio(3, 2)
```

Finally, once all the required transformations are defined, call `create` function to get the final transformed URL

```kotlin
val url = urlConstructor.create()
// https://ik.imagekit.io/your_imagekit_id/default-image.jpg?tr=h-400,ar-3-2
```

It will look as shown below. In the sample app, the buttons are present to demonstrate the use of different transformations.

![](<../../.gitbook/assets/Screenshot 2023-12-21 at 12.16.01 AM.png>)

We can also use the [extensions](#extensions-for-third-party-libraries) for the `ImagekitUrlConstructor` to create the image URLs with the image request objects from image loading libraries like Glide, Coil, Picasso etc.

## Common image manipulation in Android application

This section covers the basics:‌

* ​Basic image resizing
* Crop Mode
* ​Aspect Ratio
* ​Chained transformation

ImageKit Android SDK provides a function  to each transformation parameter e.g. `height` for `h` and `width` for `w` parameter. It makes your code more readable. See the [full list of supported transformations](https://github.com/imagekit-developer/imagekit-android#list-of-supported-transformations) in Android SDK on Github.

The complete list of transformations supported and their usage in ImageKit can be found [here](https://docs.imagekit.io/imagekit-docs/image-transformations). If a transformation is supported in ImageKit, but if a name for it cannot be found in the SDK, then use the `addCustomTransformation` function and pass the transformation code from ImageKit docs as the first parameter and value as the second parameter. For example - `.addCustomTransformation("w", "400").`

### Basic image resizing <a href="basic-image-resizing" id="basic-image-resizing"></a>

Let's resize the image to a height of 150px and a width of 150px.

```kotlin
val url = ImageKit.getInstance()
    .url(
        path = "default-image.jpg",
        transformationPosition = TransformationPosition.QUERY
    )
    .height(150)
    .width(150)
    .create()
```

Output:

![](<../../.gitbook/assets/Screenshot 2023-09-11 at 2.32.43 PM.png>)


### Crop mode

Let’s now see how different crop mode works. We will try the [`pad_resize`](https://docs.imagekit.io/features/image-transformations/resize-crop-and-other-transformations#pad-resize-crop-strategy-cm-pad\_resize) crop strategy. In this strategy, the output image's dimension (height and width) is the same as requested, no cropping occurs, and the aspect ratio is preserved. This is accomplished by adding padding around the output image to get it to match the exact dimension as requested. You can read more about this [here](https://docs.imagekit.io/features/image-transformations/resize-crop-and-other-transformations#pad-resize-crop-strategy-cm-pad\_resize).

```kotlin
val url = ImageKit.getInstance()
    .url(
        path = "default-image.jpg",
        transformationPosition = TransformationPosition.QUERY
    )
    .height(300)
    .width(200)
    .cropMode(CropMode.PAD_RESIZE)
    .background("F1F1F1")
    .create()
```

Output:

![](<../../.gitbook/assets/Screenshot 2023-09-11 at 2.39.52 PM.png>)

### Aspect ratio

You can use the [ar parameter](https://docs.imagekit.io/features/image-transformations/resize-crop-and-other-transformations#aspect-ratio-ar) to change the aspect ratio like this**:**

```kotlin
val url = ImageKit.getInstance()
    .url(
        path = "default-image.jpg",
        transformationPosition = TransformationPosition.QUERY
    )
    .height(600)
    .aspectRatio(3, 2)
    .create()
```

Output:

![](<../../.gitbook/assets/Screenshot 2023-09-11 at 2.42.21 PM.png>)

### Chained transformation

[Chained transformations](https://docs.imagekit.io/features/image-transformations/chained-transformations) provide a simple way to control the sequence in which transformations are applied.

Let’s try it out by resizing an image, then [rotating](../../features/image-transformations/resize-crop-and-other-transformations.md#rotate-rt) it:

```kotlin
val url = ImageKit.getInstance()
    .url(
        path = "default-image.jpg",
        transformationPosition = TransformationPosition.QUERY
    )
    .height(400)
    .width(300)
    .chainTransformation()
    .aspectRatio(3, 2)
    .rotation(Rotation.VALUE_90)
    .create()
```

Output:

![](<../../.gitbook/assets/Screenshot 2023-09-11 at 2.44.30 PM.png>)

## **Client-side file uploading**

Let's learn how to upload an image to our media library.

Android SDK provides `ImageKitUploader` which provide functions to allow upload files to the [ImageKit media library](../../media-library/overview/) directly from the client-side.

For using upload functionality, we need to pass `publicKey` while [initializing the SDK](Android.md#setup-imagekit-android-sdk), and pass the client-generated JSON Web Token (JWT) in the upload method, which the ImageKit.io server uses to authenticate and check that the upload request parameters have not been tampered with after the generation of the token. Refer this [guide](https://docs.imagekit.io/api-reference/upload-file-api/secure-client-side-file-upload#how-to-implement-authenticationendpoint-endpoint) to create the token below on the page.

```kotlin
ImageKit.init(
    publicKey: "your_public_api_key", //Replace this with the public API key from your ImageKit account
    urlEndpoint: "your_url_endpoint", //Replace this with the URL endpoint for your ImageKit account
    transformationPosition: TransformationPosition.PATH, 
)
```

For this, we would need a dummy backend app to authenticate our upload request. API authentication for upload always happens on the backend for security reasons.

The tutorial repository comes with a sample backend server that we can use.

### **Upload a file**

To upload an image to the media library, the ImageKit Android SDK provides `ImageKitUploader` . For complete API reference for the image upload function, check out the [docs](https://github.com/imagekit-developer/imagekit-Android#file-upload) on the ImageKit Android Git Repository.

The `ImageKit.getInstance().uploader().upload` function can ingest files through `Bitmap`, `File` or URL of a remote image.

```kotlin
ImageKit.getInstance().uploader().upload(
    file = image,
    token = "UPLOAD_JWT" //Replace this with the JWT returned by your custom auth server
    fileName = "sample-image.jpg",
    useUniqueFilename = true,
    tags = ["demo"],
    folder = "/",
    isPrivateFile =  false,
    customCoordinates = "",
    responseFields = "",
    extensions = listOf(mapOf("name" to "google-auto-tagging", "minConfidence" to 80, "maxTags" to 10)),
    webhookUrl = "https://abc.xyz",
    overwriteFile = false,
    overwriteAITags = true,
    overwriteTags = true,
    overwriteCustomMetadata = true,
    customMetadata = mapOf("SKU" to "VS882HJ2JD", "price" to 599.99, "brand" to "H&M"),
    imageKitCallback = object: ImageKitCallback {
        override fun onSuccess(uploadResponse: UploadResponse) {
            // Handle Success Response
        }
        override fun onError(uploadError: UploadError) {
            // Handle Upload Error
        }
    }
)
```

After a successful upload, you should see the newly uploaded file in the [Media Library](http://dev.imagekit.io/dashboard#media-library) of your ImageKit dashboard.

If you don't see the file, check if there are any errors in the error log. Make sure that the private API key has been configured. The server app is running. And the uploaded file type is [supported](../../api-reference/upload-file-api/#allowed-file-types-for-uploading) by ImageKit.

### **Upload policy**

The [UploadPolicy](https://github.com/imagekit-developer/imagekit-android/blob/master/README.md#uploadpolicy) class represents a set of conditions that need to be met for an upload request to be executed. This policy is useful in various cases, like constraining large uploads to be performed only when the specified network and battery conditions are satisfied, and to control the retry mechanism for failed upload requests.

`UploadPolicy.Builder` class is responsible for building the `UploadPolicy` instances. You can set a default upload policy for all uploads while instantiating the SDK, e. g.:

```kotlin
ImageKit.init(
    context = applicationContext,
    publicKey = "your_public_api_key",
    urlEndpoint = "https://ik.imagekit.io/your_imagekit_id",
    transformationPosition = TransformationPosition.PATH,
    defaultUploadPolicy = UploadPolicy.Builder()
        .requireNetworkType(UploadPolicy.NetworkType.ANY)
        .setMaxRetries(3)
        .build()
)
```

And you can pass a custom upload policy in the `upload()` method to override the default policy for a particular upload, e. g.:

```kotlin
ImageKit.getInstance().uploader().upload(
    file = image,
    fileName = "sample-image.jpg",
    useUniqueFilename = true,
    policy = UploadPolicy.Builder()
        .requireNetworkType(UploadPolicy.NetworkType.UNMETERED)
        .setMaxRetries(4)
        .build(),
    imageKitCallback = object: ImageKitCallback {
        override fun onSuccess(uploadResponse: UploadResponse) {
            // Handle Success Response
        }
        override fun onError(uploadError: UploadError) {
            // Handle Upload Error
        }
    }
)
```

### **Upload preprocessors**

The `ImageKitUploader` can also perform the preprocessing of the image/video to modify them before uploading, by passing an instance of [ImageUploadPreprocessor](https://github.com/imagekit-developer/imagekit-android/blob/master/README.md#image-preprocessing)/[VideoUploadPreprocessor](https://github.com/imagekit-developer/imagekit-android/blob/master/README.md#video-preprocessing). These preprocessors can be useful for certain use cases, e.g:

- Modifying the media size and/or quality, to perhaps optimize the use of ImageKit storage.
- Applying certain media transformations from within the app, like when using an image editing feature of the app.

Currently, follwing transformations can be applied by upload preprocessors:

- Images:
    - Limiting width & height
    - Rotation
    - Crop
    - Format selection
- Videos:
    - Limiting width & height
    - Frame rate
    - A/V bitrate
    - Keyframes interval

Sample Usage
```kotlin
ImageKit.getInstance().uploader().upload(
    file = image,
    fileName = "sample-image.jpg",
    useUniqueFilename = true,
    tags = ["demo"],
    preprocessor = ImageUploadPreprocessor.Builder()
        .limit(512, 512)
        .rotate(90f)
        .build(),
    imageKitCallback = object: ImageKitCallback {
        override fun onSuccess(uploadResponse: UploadResponse) {
            // Handle Success Response
        }
        override fun onError(uploadError: UploadError) {
            // Handle Upload Error
        }
    }
)
```

## Extensions for third-party libraries

ImageKit Android SDK also provides library extensions for integrations with the following image loading libraries for Android:

- [Glide](https://github.com/imagekit-developer/imagekit-android/blob/master/README.md#glide)
- [Picasso](https://github.com/imagekit-developer/imagekit-android/blob/master/README.md#picasso)
- [Coil](https://github.com/imagekit-developer/imagekit-android/blob/master/README.md#coil)
- [Fresco](https://github.com/imagekit-developer/imagekit-android/blob/master/README.md#fresco)

## What's next

The possibilities for image manipulation and optimization with ImageKit are endless. Learn more about it here:

* [Image Transformations](https://docs.imagekit.io/features/image-transformations)
* [Image optimization](https://docs.imagekit.io/features/image-optimization)
* [Media Library](https://docs.imagekit.io/media-library/overview)
* [Performance monitoring](../../features/performance-monitoring.md)
