---
description: >-
  Real-time image resizing, automatic optimization, and file uploading in
  iOS using ImageKit.io.
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

![](<../../.gitbook/assets/Screenshot 2020-11-02 at 8.15.32 PM.png>)

You should see the following screen. This means the sample app has been set up correctly.

![](<../../.gitbook/assets/Screenshot 2020-11-02 at 8.22.01 PM.png>)

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
    pod 'ImageKitIO', '~> 2.0.0'
    ...
end
```

#### Initializing the SDK

Open `AppDelegate.swift` file, this is where we will initialize our SDK with the following parameters.

* `urlEndpoint` is the required parameter. You can get the value of URL-endpoint from your ImageKit dashboard - [https://imagekit.io/dashboard#url-endpoints](https://imagekit.io/dashboard#url-endpoints).
* `publicKey` and `authenticationEndpoint` parameters are optional and only needed if you want to use the SDK for client-side file upload. You can get these parameters from the developer section in your ImageKit dashboard - [https://imagekit.io/dashboard#developers](https://imagekit.io/dashboard#developers).

```swift
ImageKit.init(
    publicKey: "your_public_api_key=", 
    urlEndpoint: "https://ik.imagekit.io/your_imagekit_id", 
    transformationPosition: TransformationPosition.PATH, 
    authenticationEndpoint: "http://www.yourserver.com/auth"
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

It will look as shown below. In the sample app, the buttons are present to demonstrate the use of different transformations. \


![](<../../.gitbook/assets/Screenshot 2020-11-03 at 1.45.39 PM.png>)

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
urlConstructor = urlConstructor.height(height: 400)
let url = urlConstructor.create()
```

Output:

![](<../../.gitbook/assets/Screenshot 2020-11-03 at 1.51.37 PM.png>)

### Crop mode

Let’s now see how different crop mode works. We will try the [`pad_resize`](https://docs.imagekit.io/features/image-transformations/resize-crop-and-other-transformations#pad-resize-crop-strategy-cm-pad\_resize) crop strategy. In this strategy, the output image's dimension (height and width) is the same as requested, no cropping occurs, and the aspect ratio is preserved. This is accomplished by adding padding around the output image to get it to match the exact dimension as requested. You can read more about this [here](https://docs.imagekit.io/features/image-transformations/resize-crop-and-other-transformations#pad-resize-crop-strategy-cm-pad\_resize).

```swift
urlConstructor = urlConstructor.width(width: 300)
urlConstructor = urlConstructor.height(height: 200)
urlConstructor = urlConstructor.cropMode(cropMode: CropMode.PAD_RESIZE)
let url = urlConstructor.create()
```

Output:

![](<../../.gitbook/assets/Screenshot 2020-11-03 at 2.08.01 PM.png>)

### Aspect ratio

You can use the [ar parameter](https://docs.imagekit.io/features/image-transformations/resize-crop-and-other-transformations#aspect-ratio-ar) to change the aspect ratio like this**:**

```swift
urlConstructor = urlConstructor.height(height: 400)
urlConstructor = urlConstructor.aspectRatio(width: 3, height: 2)
let url = urlConstructor.create()
```

Output:

![](<../../.gitbook/assets/Screenshot 2020-11-03 at 2.10.38 PM.png>)

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

![](<../../.gitbook/assets/Screenshot 2020-11-03 at 2.12.07 PM.png>)

## **Adding an overlay to images in iOS application**

ImageKit.io allows you to add [text](https://docs.imagekit.io/features/image-transformations/overlay#text-overlay) and [image overlay](https://docs.imagekit.io/features/image-transformations/overlay) dynamically.

### Text overlay <a href="text-overlay" id="text-overlay"></a>

Text overlay can be used to superimpose text on an image. Try it like so:

```swift
urlConstructor = urlConstructor.width(width: "400")
urlConstructor = urlConstructor.overlayText(overlayText: "Hand with a green plant")
urlConstructor = urlConstructor.overlayTextColor(overlayTextColor: "264120")
urlConstructor = urlConstructor.overlayTextFontSize(overlayTextSize: 30)
urlConstructor = urlConstructor.overlayX(overlayX: 10)
urlConstructor = urlConstructor.overlayY(overlayY: 20)
let url = urlConstructor.create()
```

Output:

![](<../../.gitbook/assets/Screenshot 2020-11-03 at 3.29.26 PM.png>)

### Image overlay

Image overlay can be used like this:

```swift
urlConstructor = urlConstructor.overlayImage(overlayImage: "logo-white_SJwqB4Nfe.png")
urlConstructor = urlConstructor.overlayX(overlayX: 10)
urlConstructor = urlConstructor.overlayY(overlayY: 20)
let url = urlConstructor.create()
```

Output:

![](<../../.gitbook/assets/Screenshot 2020-11-03 at 3.41.13 PM.png>)

## **Client-side file uploading**

Let's learn how to upload an image to our media library.

iOS SDK provides `ImageKitUploader` which provide functions to allow upload files to the [ImageKit media library](../../media-library/overview/) directly from the client-side.&#x20;

For using upload functionality, we need to pass `publicKey` and `authenticationEndpoint` while [initializing the SDK](ios.md#setup-imagekit-ios-sdk).  Replace `your_url_endpoint` , `your_public_key`, `your_authentication_endpoint` with actual values.

```bash
ImageKit.init(
    publicKey: "your_public_api_key", 
    urlEndpoint: "your_url_endpoint", 
    transformationPosition: TransformationPosition.PATH, 
    authenticationEndpoint: "your_authentication_endpoint"
)
```

\
For this, we would need a dummy backend app to authenticate our upload request. API authentication for upload always happens on the backend for security reasons.

The tutorial repository comes with a sample backend server that we can use.&#x20;

### **Setting up the backend app**

For this quick start guide, we have provided the sample implementation of the authentication endpoint in using the [ImageKit Node.js SDK](https://github.com/imagekit-developer/imagekit-nodejs) and [Express](https://expressjs.com).

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
const app = express();

var cors = require('cors');
app.use(cors());

const ImageKit = require('imagekit');
const imagekit = new ImageKit({ 
  privateKey: "your_private_key", 
  publicKey: "your_public_key", 
  urlEndpoint: "your_url_endpoint"
})

app.get("/auth", function (req, res) {
  var signature = imagekit.getAuthenticationParameters();
  res.status(200);
  res.send(signature);
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

If you run `curl http://localhost:8080/auth` in the terminal, you should see a JSON response like this. Actual values will vary.

```javascript
{
    token: "5dd0e211-8d67-452e-9acd-954c0bd53a1f",
    expire: 1601047259,
    signature: "dcb8e72e2b6e98186ec56c62c9e62886f40eaa96"
}
```

**Configure the auth endpoint in the frontend app**

Head over to `AppDelegate.swift` and ensure the** **`authenticationEndpoint` is set to `http://localhost:8080/auth`

![](<../../.gitbook/assets/Screenshot 2020-11-10 at 12.49.24 PM.png>)

### **Upload a file**

To upload an image to the media library, the ImageKit iOS SDK provides `ImageKitUploader` . For complete API reference for the image upload function, check out the [docs](https://github.com/imagekit-developer/imagekit-ios#file-upload) on the ImageKit iOS Git Repository.&#x20;

The `ImageKit.shared.uploader().upload` function can ingest files through `UIImage`, `NSData` or `Url of a remote image`

```bash
ImageKit.shared.uploader().upload(
  file: image,
  fileName: "sample-image.jpg",
  useUniqueFilename: true,
  tags: ["demo"],
  folder: "/",
  isPrivateFile: false,
  customCoordinates: "",
  responseFields: "",
  progress: { progress in
  	// Handle Progress
  },
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

## What's next

The possibilities for image manipulation and optimization with ImageKit are endless. Learn more about it here:&#x20;

* [Image Transformations](https://docs.imagekit.io/features/image-transformations)
* [Image optimization](https://docs.imagekit.io/features/image-optimization)
* [Media Library](https://docs.imagekit.io/media-library/overview)
* [Performance monitoring](../../features/performance-monitoring.md)
