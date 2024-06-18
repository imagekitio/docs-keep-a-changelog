---
description: >-
  Real-time image resizing, automatic optimization, and file uploading in
  Android using ImageKit.io.
---

# iOS

This is a quick start guide to show you how to integrate ImageKit in an iOS application. The code samples covered here are hosted on Github: [https://github.com/imagekit-samples/quickstart/tree/master/ios](https://github.com/imagekit-samples/quickstart/tree/master/ios).

This guide walks you through the following topics: ‌

* [Clone and run the tutorial app](ios.md#clone-and-run-the-tutorial-app)
* [Setting up ImageKit iOS SDK ](ios.md#setup-imagekit-ios-sdk)
* [Rendering images ](ios.md#rendering-images-in-ios-application)
* [Applying common image manipulations ](ios.md#common-image-manipulation-in-ios-application)
* [Adding overlays to images](ios.md#adding-an-overlay-to-images-in-ios-application)
* [Client-side file uploading ](ios.md#client-side-file-uploading)
* [Setting a upload policy](ios.md#setting-a-upload-policy)
* [How to preprocess your files before uploading](ios.md#how-to-preprocess-your-files-before-uploading)

## **Clone and run the tutorial app**

For this tutorial, it is recommended to use the sample iOS app, as shown below. If you already have an existing iOS app, it is also possible to use that, which will be explained in the continuation of the sample app.

```bash
git clone 
https://github.com/imagekit-samples/quickstart.git
```

Navigate to the cloned repository, and install the dependencies that are needed to run the iOS app:

```bash
cd quickstart/ios/
pod install
```

{% hint style="warning" %}
If you do not have CocoaPods Installed on your Mac, you can learn about setting up the development environment here [https://guides.cocoapods.org/using/getting-started.html](https://guides.cocoapods.org/using/getting-started.html).&#x20;
{% endhint %}

Once the dependencies have been installed, run the following command to open the Xcode Workspace:

```bash
open ImagekitDemo.xcodeworkspace
```

Select the emulator from the dropdown. Then run the app by clicking on the Run button or by pressing ⌘ + R

![](<../../.gitbook/assets/ios/xcode.png>)

You should see the following screen. This means the sample app has been set up correctly.

![](<../../.gitbook/assets/ios/sample.png>)

## **Setup ImageKit iOS SDK**

We will be installing [ImageKit iOS SDK](https://github.com/imagekit-developer/imagekit-ios) using [CocoaPods](https://guides.cocoapods.org/using/getting-started.html).

#### Initialising CocoaPods in your iOS Project

If you are not using CocoaPods in your project, you might need to add it to your project by either running `pod init` or adding PodFile ([see example](https://guides.cocoapods.org/using/using-cocoapods.html)) in the root directory of your project.

#### Installing the SDK

To Install the SDK, add the following to the PodFile.

```bash
# Required to enable the use for Dynamic Libraries
use_frameworks!

target 'target_name' do 
    ...
    # Add to your project target
    pod 'ImageKitIO', '~> 3.0.0'
    ...
end
```

#### Initializing the SDK

Open `AppDelegate.swift` file, this is where we will initialize our SDK with the following parameters.

* `urlEndpoint` is the required parameter. You can get the value of URL-endpoint from your ImageKit dashboard - [https://imagekit.io/dashboard/url-endpoints](https://imagekit.io/dashboard/url-endpoints).
* `publicKey` is an optional parameter and only needed if you want to use the SDK for client-side file upload. You can get the public key from the developer section in your ImageKit dashboard - [https://imagekit.io/dashboard/developer/api-keys](https://imagekit.io/dashboard/developer/api-keys).

```swift
ImageKit.init(
    publicKey: "your_public_api_key", 
    urlEndpoint: "https://ik.imagekit.io/your_imagekit_id", 
    transformationPosition: TransformationPosition.PATH
)
```

## Rendering Images in iOS application

Image URL can be created from an image path or using the absolute image URL. You can learn more about it in [docs](https://github.com/imagekit-developer/imagekit-javascript#url-generation).

To render an image using an absolute URL (full image URL), we can instantiate the `ImageKitURLConstructor`

```swift
let urlConstructor = ImageKit.shared.url(
    src: "https://ik.imagekit.io/demo/default-image.jpg"
)
```

To create a URL from the image path, we can instantiate the `ImageKitURLConstructor` which takes the URL endpoint, image path, and transformation position as parameters to create the transformed url.

```swift
let urlConstructor = ImageKit.shared.url(
    urlEndpoint: "https://ik.imagekit.io/demo"
    path: "default-image.jpg",
    transformationPosition: TransformationPosition.QUERY
)
```

{% hint style="info" %}
The transformation position (path or query) is only valid when creating a URL from the image path. Transformations are always added as query parameters if the URL is created from an absolute image path using **src**.
{% endhint %}

The iOS SDK provides a [function](https://github.com/imagekit-developer/imagekit-ios#list-of-supported-transformations) for each transformation parameter, making the code simpler and readable. To add transformations, the functions can be chained with `urlConstructor`.&#x20;

```swift
urlConstructor = urlConstructor.height(height: 400)
urlConstructor = urlConstructor.aspectRatio(width: 3, height: 2)
```

Finally, once all the required transformations are defined, call `create` function to get the final transformed URL

```swift
let url = urlConstructor.create()
// https://ik.imagekit.io/your_imagekit_id/default-image.jpg?tr=h-400,ar-3-2
```

It will look as shown below. In the sample app, the buttons are present to demonstrate the use of different transformations. 


![](<../../.gitbook/assets/ios/sample-url-1.png>)

## Common image manipulation in iOS application

This section covers the basics:‌

* ​Basic image resizing
* Crop Mode
* ​Aspect Ratio
* ​Chained transformation

ImageKit iOS SDK provides a function  to each transformation parameter e.g. `height` for `h` and `width` for `w` parameter. It makes your code more readable. See the [full list of supported transformations](https://github.com/imagekit-developer/imagekit-ios#list-of-supported-transformations) in iOS SDK on Github.

The complete list of transformations supported and their usage in ImageKit can be found [here](https://docs.imagekit.io/imagekit-docs/image-transformations). If a transformation is supported in ImageKit, but if a name for it cannot be found in the SDK, then use the `addCustomTransformation` function and pass the transformation code from ImageKit docs as the first parameter and value as the second parameter. For example - `.addCustomTransformation("w", "400").`

### Basic image resizing <a href="basic-image-resizing" id="basic-image-resizing"></a>

Let's resize the image to a height of 150 and a width of 150.

```swift
urlConstructor = urlConstructor.height(height: 150)
urlConstructor = urlConstructor.width(width: 400)
let url = urlConstructor.create()
```

Output:

![](<../../.gitbook/assets/ios/sample-url-2.png>)

### Crop mode

Let’s now see how different crop mode works. We will try the [`pad_resize`](https://docs.imagekit.io/features/image-transformations/resize-crop-and-other-transformations#pad-resize-crop-strategy-cm-pad\_resize) crop strategy. In this strategy, the output image's dimension (height and width) is the same as requested, no cropping occurs, and the aspect ratio is preserved. This is accomplished by adding padding around the output image to get it to match the exact dimension as requested. You can read more about this [here](https://docs.imagekit.io/features/image-transformations/resize-crop-and-other-transformations#pad-resize-crop-strategy-cm-pad\_resize).

```swift
urlConstructor = urlConstructor.width(width: 300)
urlConstructor = urlConstructor.height(height: 200)
urlConstructor = urlConstructor.cropMode(cropMode: CropMode.PAD_RESIZE)
let url = urlConstructor.create()
```

Output:

![](<../../.gitbook/assets/ios/sample-url-3.png>)

### Aspect ratio

You can use the [ar parameter](https://docs.imagekit.io/features/image-transformations/resize-crop-and-other-transformations#aspect-ratio-ar) to change the aspect ratio like this:

```swift
urlConstructor = urlConstructor.height(height: 400)
urlConstructor = urlConstructor.aspectRatio(width: 3, height: 2)
let url = urlConstructor.create()
```

Output:

![](<../../.gitbook/assets/ios/sample-url-1.png>)

### Chained transformation

[Chained transformations](https://docs.imagekit.io/features/image-transformations/chained-transformations) provide a simple way to control the sequence in which transformations are applied.

Let’s try it out by resizing an image, then [rotating](../../features/image-transformations/resize-crop-and-other-transformations.md#rotate-rt) it:

```swift
urlConstructor = urlConstructor.height(height: 400)
urlConstructor = urlConstructor.width(width: 300)
urlConstructor = urlConstructor.chainTransformation()
urlConstructor = urlConstructor.rotation(rotation: Rotation.VALUE_90)
let url = urlConstructor.create()
```

Output:

![](<../../.gitbook/assets/ios/sample-url-4.png>)

## **Adding an overlay to images in iOS application**

ImageKit.io allows you to add [text](../../features/image-transformations/overlay-using-layers.md#add-text-over-image) and [image](../../features/image-transformations/overlay-using-layers.md#transformation-of-image-overlay) dynamically ysing layers.

### Text overlay <a href="text-overlay" id="text-overlay"></a>

Text overlay can be used to superimpose text on an image. Try it like so:

```swift
urlConstructor = urlConstructor.width(width: "400")
urlConstructor = urlConstructor.raw(params: "l-text,i-Hand%20with%20a%20green%20plant,co-264120,fs-30,lx-10,ly-20,l-end")
let url = urlConstructor.create()
```

Output:

![](<../../.gitbook/assets/ios/sample-url-5.png>)

### Image overlay

Image overlay can be used like this:

```swift
urlConstructor = urlConstructor.raw(params: "l-image,i-logo-white_SJwqB4Nfe.png,lx-10,ly-20,l-end")
let url = urlConstructor.create()
```

Output:

![](<../../.gitbook/assets/ios/sample-url-6.png>)

## **Client-side file uploading**

Let's learn how to upload an image to our media library.

iOS SDK provides `ImageKitUploader` which provide functions to allow upload files to the [ImageKit media library](../../media-library/overview/) directly from the client-side.&#x20;

For using upload functionality, we need to pass `publicKey` while [initializing the SDK](ios.md#setup-imagekit-ios-sdk). 
Replace `your_url_endpoint` and `your_public_key` with actual values.

```bash
ImageKit.init(
    publicKey: "your_public_api_key", 
    urlEndpoint: "your_url_endpoint", 
    transformationPosition: TransformationPosition.PATH
)
```


For this, we would need a dummy backend app to authenticate our upload request. API authentication for upload always happens on the backend for security reasons.

The tutorial repository comes with a sample backend server that we can use.

### **Setting up the backend app**

For this quick start guide, we have provided the sample implementation of the authentication endpoint in using [jsonwebtoken](https://www.npmjs.com/package/jsonwebtoken) and [Express](https://expressjs.com).

In a new terminal window, navigate to the `Server` folder inside the tutorial project and install its npm packages:

```bash
cd Server/
npm install
```

Let's modify `index.js` to implement `http://localhost:8080/auth` which is our `authenticationEndpoint` .

```javascript
/* 
    This is our backend server.
    Replace your_url_endpoint, your_public_key, your_private_key 
    with actual values
*/
const express = require('express');
const jwt = require('jsonwebtoken');
const app = express();
const cors = require('cors');

app.use(cors());
app.use(express.json());

const publicKey = "your_public_key";
const privateKey = "your_private_key";

app.post("/auth", function (req, res) {
  const token = jwt.sign(
    req.body.uploadPayload,
    privateKey,
    {
      expiresIn: req.body.expire,
      header: {
        alg: "HS256",
        typ: "JWT",
        kid: publicKey,
      },
    })
  res.status(200);
  res.send({ token });
});

app.listen(8080, function () {
  console.log("Live at Port 8080");
});
```

Finally, run the backend app.

```bash
node index.js
```

You should see a log line saying that the app is _**“Live on port 8080”**_.

If you run the following command in the terminal

```shell
curl -L 'localhost:8080/auth' -H 'Content-Type: application/json' -d '{
    "uploadPayload":{
        "filename": "image.jpg",
        "folder": "/"
    },
    "expire": 60
}'
```

You should see a JSON response like this which contains the JWT token. Actual values will vary.

```javascript
{
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCIsImtpZCI6InRlc3QifQ.eyJmaWxlbmFtZSI6ImltYWdlLmpwZyIsImZvbGRlciI6Ii8iLCJpYXQiOjE3MTg2OTUwNzUsImV4cCI6MTcxODY5NTEzNX0.HN_0gHfI26drmFGHmIiZuLnz21nzz6VpTuRWUi4Ljno"
}
```

You can use [JWT Debugger](https://jwt.io/) to decode and verify that the token is correctly generated.

![](<../../.gitbook/assets/ios/jwt.png>)

**Configure the auth endpoint in the frontend app**

Head over to `Constants.swift` and ensure the** **`AUTH_SERVER_API_ENDPOINT` is set to `http://localhost:8080/auth`

![](<../../.gitbook/assets/ios/xcode-constants.png>)

### **Upload a file**

To upload an image to the media library, the ImageKit iOS SDK provides `ImageKitUploader` . For complete API reference for the image upload function, check out the [docs](https://github.com/imagekit-developer/imagekit-ios#file-upload) on the ImageKit iOS Git Repository.&#x20;

The `ImageKit.shared.uploader().upload` function can ingest files through `UIImage`, `NSData` or `Url of a remote image`

```bash
ImageKit.shared.uploader().upload(
  file: image,
  token: "your_jwt_token",
  fileName: "sample-image.jpg",
  useUniqueFilename: true,
  tags: ["demo"],
  folder: "/",
  isPrivateFile: false,
  customCoordinates: "",
  responseFields: "",
  overwriteFile: false,
  overwriteAITags: false,
  overwriteTags: false,
  overwriteCustomMetadata: false,
  // Custom metadata must be defined in the Media Library before using it in the upload API
  // customMetadata: "{\"device_name\":\"iPhone 15\",\"location\":\"New York\"}",
  progress: { progress in
  	// Handle Progress
  },
  policy: UploadPolicy,
  preProcessor: UploadPreprocessor,
  completion: { result in 
  	 switch result{
            case .success(let uploadAPIResponse):
                // Handle Success Response
            case .failure(let error as UploadAPIError):
                // Handle Upload Error
            case .failure(let error):
                // Handle Other Errors
      }
  }
)
```

After a successful upload, you should see the newly uploaded file in the [Media Library](http://dev.imagekit.io/dashboard#media-library) of your ImageKit dashboard.

If you don't see the file, check if there are any errors in the error log. Make sure that the private API key has been configured. The server app is running. And the uploaded file type is [supported](../../api-reference/upload-file-api/#allowed-file-types-for-uploading) by ImageKit.

## **Setting a upload policy**

The `UploadPolicy` struct is used to define a set of conditions that need to be met for a file to be uploaded. For complete API reference for the upload policy, check out the [docs](https://github.com/imagekit-developer/imagekit-ios?tab=readme-ov-file#uploadpolicy) on the ImageKit iOS Git Repository.

```swift
/*
 * Upload policy to upload files only 
 *  - when the network is unmetered
 *  - the device doesn't require charging
 *  - retry 4 times with exponential backoff starting at 500ms
*/

let uploadPolicy = UploadPolicy.Builder()
  .requireNetworkType(.UNMETERED)
  .requiresBatteryCharging(false)
  .maxRetries(4)
  .backoffCriteria(backoffMillis: 500, backoffPolicy: .EXPONENTIAL)
  .build()
```

## **How to preprocess your files before uploading**

The ImageKit iOS SDK provides a way to preprocess files before uploading them to the media library. The `UploadPreprocessor` protocol is used to define a set of transformations that need to be applied to the file before uploading. For complete API reference for the upload preprocessor, check out the [docs](https://github.com/imagekit-developer/imagekit-ios?tab=readme-ov-file#upload-preprocessing)

The SDK features two built-in preprocessors:

- `ImageUploadPreprocessor`: This preprocessor is used to apply transformations to images before uploading them to the media library.

```swift
let preprocessor = ImageUploadPreprocessor.Builder()
   .limit(width: 400, height: 300)
   .rotate(degrees: 45)
   .format(format: .JPEG)
   .build()
```

- `VideoUploadPreprocessor`: This preprocessor is used to apply transformations to videos before uploading them to the media library.

```swift
let preprocessor = VideoUploadPreprocessor.Builder()
   .limit(width: 800, height: 600)
   .frameRate(frameRateValue: 60)
   .keyFramesInterval(interval: 6)
   .targetVideoBitrateKbps(targetVideoBitrateKbps: 480)
   .targetAudioBitrateKbps(targetAudioBitrateKbps: 320)
   .build(),
```

## What's next

The possibilities for image manipulation and optimization with ImageKit are endless. Learn more about it here:&#x20;

* [Image Transformations](https://docs.imagekit.io/features/image-transformations)
* [Image optimization](https://docs.imagekit.io/features/image-optimization)
* [Media Library](https://docs.imagekit.io/media-library/overview)
* [Performance monitoring](../../features/performance-monitoring.md)
