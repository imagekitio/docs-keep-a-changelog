---
description: >-
  Real-time image resizing, automatic optimization, and file uploading in
  Android using ImageKit.io.
---

# Android

This is a quick start guide to show you how to integrate ImageKit in an Android application.

#### Installing the SDK

To Install the SDK, add the following to the root-level `build.gradle`:
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

Initialize our SDK prefereable in the `Application` class or any `Activity` as needed, with the following code:

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
