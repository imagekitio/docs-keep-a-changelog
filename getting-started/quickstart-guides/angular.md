---
description: >-
  Real-time image resizing, automatic optimization, and file uploading in Angular
  using ImageKit.io.
---

# Angular

This quick start guide shows you how to integrate ImageKit into an Angular application. The code samples covered here are hosted on GitHub: [https://github.com/imagekit-samples/quickstart/tree/master/angular](https://github.com/imagekit-samples/quickstart/tree/master/angular).

This guide walks you through the following topics: ‌

* [Setting up ImageKit Angular SDK](angular.md#setup-imagekit-angular-sdk)
* [Rendering images](angular.md#rendering-images)
* [Applying common image manipulations](angular.md#basic-image-manipulation)
* [Adding overlays to images](angular.md#adding-overlays-to-images)
* [Lazy loading images](angular.md#lazy-loading-images-in-angular)
* [Blurred image placeholder](angular.md#blurred-image-placeholder)
* [Client-side file uploading](angular.md#uploading-files-in-angular)
* [Advanced file uploading](angular.md#advanced-file-upload)
* [Rendering videos](angular.md#rendering-videos)

## **Setup ImageKit Angular SDK**

For this tutorial, it is recommended to create an Angular dummy app, as shown below.

**Create an Angular app:**

We will be using the following in this guide:
- Node version 14.21.1
- Angular version 15

Let's use the `ng new <project name>` CLI utility provided by Angular to build a new project:

```bash
ng new imagekit-angular-app
```

Navigate to the project directory:

```
cd imagekit-angular-app/
```

Open up the project in your text editor of choice, and navigate to `src/app/`. This is where we will do most of our work.


Install libraries (if not already):

```
npm install
```

Now run the app:

```
npm start
```

In your web browser, navigate to `http://localhost:4200/`

You should see the dummy app created by Angular CLI. 

For simplicity, let's remove everything from `src/app/app.component.html` so we can begin with a clean slate.

Let's add one line in `src/app/app.component.html` to title our page :

```jsx
<h1>Imagekit Angular Demo</h1>
```

Now we can begin our work.

**Install the ImageKit Angular SDK:**

Installing the ImageKit Angular SDK in our app is pretty simple:

```
npm install --save imagekitio-angular
```

#### **Initialize the Angular SDK:**

Before the SDK can be used, let's learn about and obtain the requisite initialization parameters:

* `urlEndpoint` is a required parameter. This can be obtained from the [URL-endpoint section](https://imagekit.io/dashboard/url-endpoints) or the [developer section](https://imagekit.io/dashboard/developer/api-keys) on your ImageKit dashboard.
* `publicKey` and `authenticator` parameters are optional and only needed if you want to use the SDK for client-side file upload. `publicKey` can be obtained from the [developer section](https://imagekit.io/dashboard/developer/api-keys) on your ImageKit dashboard.
* `authenticator` expects an asynchronous function that resolves with an object containing the necessary security parameters i.e signature, token, and expire.

> Note: Do not include your [private key](https://docs.imagekit.io/api-reference/api-introduction/api-keys#private-key) in any client-side code.

**ImageKit Components:**

The SDK includes 3 Components and the ability to access the core component:

* [ik-image](https://github.com/imagekit-developer/imagekit-angular#ik-image) for image resizing. This renders a `<img>` tag.

* [ik-video](https://github.com/imagekit-developer/imagekit-angular#ik-video) for video resizing. This renders a `<video>` tag.

* [ik-upload](https://github.com/imagekit-developer/imagekit-angular#ik-upload) for client-side file uploading. This renders a `<input type="file">` tag.

Accessing the underlying [ImageKit javascript SDK](https://github.com/imagekit-developer/imagekit-javascript). See 
[here](https://github.com/imagekit-developer/imagekit-angular#accessing-imagekit-core-js-sdk) for more details.

Note: URL endpoints of each component can be overridden explicitly. [See here for more details](https://github.com/imagekit-developer/imagekit-angular#overriding-urlendpoint)

#### Configure the app for ImageKit:

Let's import the SDK.

{% code title="src/app/app.module.ts" %}
```jsx
import { ImagekitioAngularModule } from 'imagekitio-angular';
...
@NgModule({
  ...
  imports: [
    ...
    ImagekitioAngularModule.forRoot({
      publicKey: environment.publicKey,
      urlEndpoint: environment.urlEndpoint,
    })
  ],
  ...
})
...
```
{% endcode %}

## **Rendering images**

**Loading image from relative path:**

Let's use the default image that we have. It should be available at the following URL:

```
https://ik.imagekit.io/demo/default-image.jpg
```

Let's fetch and display it! For this, we will use the `ik-image` component.

We use the tag `<ik-image>` for rendering images. For now, we will do a simple image rendering with a `path` prop. For a full list of options, check [here](https://github.com/imagekit-developer/imagekit-angular#ik-image)

Let's insert the following into `app.component.html`.
{% code title="src/app/app.component.html" %}
```jsx
<ik-image 
  urlEndpoint="https://ik.imagekit.io/demo/"
  path="default-image.jpg"
></ik-image>

```
{% endcode %}

**Rendered HTML element:**
```markup
<img src="https://ik.imagekit.io/demo/default-image.jpg" _ngcontent-vpo-c15="" urlendpoint="https://ik.imagekit.io/demo/" path="default-image.jpg" ng-reflect-url-endpoint="https://ik.imagekit.io/demo/" ng-reflect-path="default-image.jpg">
```

Your Angular app should now display the default image in its full size:

![Full sized image (1000px x 1000 px)](<../../.gitbook/assets/angular/angular-sdk-render-image (1).png>)

You can pass styles and other attributes as props. For e.g. let's add 400px width by adding the `transformation.width` prop:

Let's try creating a transformation object in `app.component.ts`.

{% code title="src/app/app.component.ts" %}
```jsx
import { Transformation } from 'imagekit-javascript/dist/src/interfaces/Transformation';
...
export class AppComponent {
    ...
    transformation: Array<Transformation> = [{
        width: "400"
    }];
    ...
}
```
{% endcode %}

And now, we can use it in `app.component.html` as such:

{% code title="src/app/app.component.html" %}
```jsx
<ik-image 
  urlEndpoint="https://ik.imagekit.io/demo/"
  path="default-image.jpg"
  [transformation]="transformation"
></ik-image>

```
{% endcode %}

**This is how the output should look now:**

![Resized image (width=400px)](<../../.gitbook/assets/angular/angular-sdk-render-image-resized.png>)

{% hint style="info" %}
Note that here we have set the width to 400px at the `<img>` tag level only. Intrinsically, the fetched image is still 1000px wide.
{% endhint %}

There are other transformations available - see the list of [different tranformations](https://github.com/imagekit-developer/imagekit-angular#list-of-supported-transformations)

**Loading image from an absolute path:**

If you have an absolute image path coming from the backend API e.g. `https://www.custom-domain.com/default-image.jpg`, then you can use the `src` prop to load the image.

**For example:**

```jsx
<ik-image
  src="https://ik.imagekit.io/demo/default-image.jpg"
></ik-image>
```

**The output looks like this:**

![Render image on custom domain via absolute path](<../../.gitbook/assets/angular/angular-sdk-render-image-absolute-path.png>)


## **Basic image manipulation**

Let’s now learn how to manipulate images using transformations.

* [Resizing images](angular.md#height-and-width-manipulation)
* [Quality manipulation](angular.md#quality-manipulation)
* [Crop mode](angular.md#crop-mode)
* [Chained transformation](angular.md#chained-transformation)

The Angular SDK gives a name to each transformation parameter, e.g., `height` for `h` and `width` for the `w` parameter. It makes your code more readable. If the property does not match any of the available options, it is added as it is. See the [full list of supported transformations](https://github.com/imagekit-developer/imagekit-angular#list-of-supported-transformations) in Angular SDK on GitHub.

You can also use `h` and `w` parameters instead of `height` and `width`.\
See the complete list of transformations supported in ImageKit [here](../../features/image-transformations/resize-crop-and-other-transformations.md).

### **Height and width manipulation**

‌To resize an image along with its [height](../../features/image-transformations/resize-crop-and-other-transformations.md#height-h) or [width](../../features/image-transformations/resize-crop-and-other-transformations.md#width-w), we need to pass the `transformation` object as a prop to `ik-image`.

Let’s resize the default image to 200px height and width:

{% code title="src/app/app.component.ts" %}
```jsx
    ...
    transformation: Array<Transformation> = [{
        height: "200",
        width: "200"
    }];
    ...
}
```
{% endcode %}

{% code title="src/app/app.component.html" %}
```jsx
<ik-image 
  urlEndpoint="https://ik.imagekit.io/demo/"
  path="default-image.jpg"
  [transformation]="transformation"
></ik-image>

```
{% endcode %}

**Rendered HTML element:**

```markup
<img src="https://ik.imagekit.io/demo/tr:h-400,w-400/default-image.jpg" _ngcontent-qos-c15="" urlendpoint="https://ik.imagekit.io/demo/" class="lazyload" ng-reflect-url-endpoint="https://ik.imagekit.io/demo/" ng-reflect-path="default-image.jpg" ng-reflect-transformation="[object Object]">
```

Refresh your browser to get the resized image.

![Resized Image (200x200px)](<../../.gitbook/assets/angular/angular-sdk-basic-height-width.png>)

### **Quality manipulation**

You can use the [quality parameter](../../features/image-transformations/resize-crop-and-other-transformations.md#quality-q) to change image quality like this:

{% code title="src/app/app.component.ts" %}
```jsx
    ...
    transformation: Array<Transformation> = [{
        width: "400",
        quality: "10"
    }];
    ...
}
```
{% endcode %}

**Rendered HTML:**

```markup
<img src="https://ik.imagekit.io/demo/tr:w-400,q-10/default-image.jpg" _ngcontent-hfw-c15="" urlendpoint="https://ik.imagekit.io/demo/" ng-reflect-url-endpoint="https://ik.imagekit.io/demo/" ng-reflect-path="default-image.jpg" ng-reflect-transformation="[object Object]">
```

![Quality manipulation (q=10)](<../../.gitbook/assets/angular/angular-sdk-basic-quality.png>)

### **Crop mode**‌

Let’s now see how [cropping](../../features/image-transformations/resize-crop-and-other-transformations.md#crop-crop-modes-and-focus) works. We will try the [`extract`](../../features/image-transformations/resize-crop-and-other-transformations.md#extract-crop-strategy-cm-extract) crop strategy.  In this strategy, instead of resizing the whole image, we extract a region of the requested dimension from the original image. You can read more about this [here](https://docs.imagekit.io/features/image-transformations/resize-crop-and-other-transformations#extract-crop-strategy-cm-extract).

{% code title="src/app/app.component.ts" %}
```jsx
    ...
    transformation: Array<Transformation> = [{
        height: "300", 
        width: "200",
        cropMode: "extract"
    }];
    ...
}
```
{% endcode %}

**Rendered HTML element:**

```markup
<img src="https://ik.imagekit.io/demo/tr:h-300,w-200,cm-extract/default-image.jpg" _ngcontent-vjd-c15="" urlendpoint="https://ik.imagekit.io/demo/" ng-reflect-url-endpoint="https://ik.imagekit.io/demo/" ng-reflect-path="default-image.jpg" ng-reflect-transformation="[object Object]">
```

![Crop Mode Extract (200x300px)](<../../.gitbook/assets/angular/angular-sdk-basic-crop-mode.png>)

### **Chained transformation**

[Chained transformations](https://docs.imagekit.io/features/image-transformations/chained-transformations) provide a simple way to control the sequence in which transformations are applied.

Let’s try it out by [resizing](../../features/image-transformations/resize-crop-and-other-transformations.md#basic-image-resizing) an image, then [rotating](../../features/image-transformations/resize-crop-and-other-transformations.md#rotate-rt) it:

First, we will apply resizing transformations:

{% code title="src/app/app.component.ts" %}
```jsx
    ...
    transformation: Array<Transformation> = [{
        height: "300", 
        width: "200",
    }];
    ...
}
```
{% endcode %}

**Transformation URL:**

```markup
<img src="https://ik.imagekit.io/demo/tr:h-300,w-200/default-image.jpg" _ngcontent-dsh-c15="" urlendpoint="https://ik.imagekit.io/demo/" ng-reflect-url-endpoint="https://ik.imagekit.io/demo/" ng-reflect-path="default-image.jpg" ng-reflect-transformation="[object Object]">
```

![Resized and cropped (200x300px)](<../../.gitbook/assets/angular/angular-sdk-basic-chained-1.png>)

Now, rotate the image by 90 degrees.

{% code title="src/app/app.component.ts" %}
```jsx
    ...
    transformation: Array<Transformation> = [{
        height: "300", 
        width: "200",
    }, {
        rt: "90"
    }
    ];
    ...
}
```
{% endcode %}

**Chained Transformation URL:**

```markup
<img src="https://ik.imagekit.io/demo/tr:h-300,w-200:rt-90/default-image.jpg" _ngcontent-cqk-c15="" urlendpoint="https://ik.imagekit.io/demo/" ng-reflect-url-endpoint="https://ik.imagekit.io/demo/" ng-reflect-path="default-image.jpg" ng-reflect-transformation="[object Object],[object Object">
```

![Resized, then rotated](<../../.gitbook/assets/angular/angular-sdk-basic-chained-2.png>)

Let’s flip the order of transformation and see what happens.

{% code title="src/app/app.component.ts" %}
```jsx
    ...
    transformation: Array<Transformation> = [{
        rt: "90"
    },{
        height: "300", 
        width: "200",
    }
    ];
    ...
}
```
{% endcode %}

**Chained Transformation URL:**

```markup
<img src="https://ik.imagekit.io/demo/tr:rt-90:h-300,w-200/default-image.jpg" _ngcontent-lvp-c15="" urlendpoint="https://ik.imagekit.io/demo/" ng-reflect-url-endpoint="https://ik.imagekit.io/demo/" ng-reflect-path="default-image.jpg" ng-reflect-transformation="[object Object],[object Object">
```

![Rotated, then resized](<../../.gitbook/assets/angular/angular-sdk-basic-chained-3.png>)

## **Adding overlays to images**

ImageKit.io allows you to add [text](../../features/image-transformations/overlay.md#text-overlay) and [image overlay](../../features/image-transformations/overlay.md) dynamically.

For example, a text overlay can be used to superimpose text on an image. Try it like so:

{% code title="src/app/app.component.ts" %}
```jsx
    ...
    transformation: Array<Transformation> = [{
        height: "300", 
        width: "300",
        overlayText: 'ImageKit',
        overlayTextFontSize: "50",
        overlayTextColor: '0651D5',
    }];
    ...
}
```
{% endcode %}

**Rendered HTML element:**

```markup
<img src="https://ik.imagekit.io/demo/tr:h-300,w-300,ot-ImageKit,ots-50,otc-0651D5/default-image.jpg" _ngcontent-twl-c15="" urlendpoint="https://ik.imagekit.io/demo/" ng-reflect-url-endpoint="https://ik.imagekit.io/demo/" ng-reflect-path="default-image.jpg" ng-reflect-transformation="[object Object]">
```

![Text Overlay (300x300px)](<../../.gitbook/assets/angular/angular-sdk-overlay-text.png>)

## **Lazy-loading images in Angular**

You can lazy load images using the `loading` prop in the `ik-image` component. When you use `loading="lazy"`, all images that are immediately viewable without scrolling load normally. Those that are far below the device viewport are only fetched when the user scrolls near them.

The SDK uses a fixed threshold based on the effective connection type to ensure that images are loaded early enough so that they have finished loading once the user scrolls near to them.

{% hint style="info" %}
You should always set the height and width of the image element to avoid[ layout shift](https://www.youtube.com/watch?v=4-d\_SoCHeWE) when lazy-loading images.
{% endhint %}

{% code title="src/app/app.component.ts" %}
```jsx
    ...
    transformation: Array<Transformation> = [{
        height: "300"
        width: "400"
    }];
    ...
}
```
{% endcode %}

{% hint style="info" %}
You should always set the height and width of the image element to avoid[ layout shift](https://www.youtube.com/watch?v=4-d\_SoCHeWE) when lazy-loading images.
{% endhint %}

{% code title="src/app/app.component.html" %}
```jsx
<ik-image
    path={{path}}
    urlEndpoint="https://ik.imagekit.io/demo/"
    [transformation]="transformation"
    loading="lazy"
    height="300"
    width="400"
    >
  </ik-image>
```
{% endcode %}

**Rendered HTML element:**

```markup
<img src="https://ik.imagekit.io/demo/tr:h-300,w-400/default-image.jpg" _ngcontent-vnv-c15="" urlendpoint="https://ik.imagekit.io/demo/" loading="lazy" ng-reflect-url-endpoint="https://ik.imagekit.io/demo/" ng-reflect-loading="lazy" ng-reflect-path="default-image.jpg" ng-reflect-transformation="[object Object]">
```

## **Blurred image placeholder**

To improve user experience, you can use a low-quality [blurred](../../features/image-transformations/resize-crop-and-other-transformations.md#blur-bl) variant of the original image as a placeholder while the original image is being loaded in the background. Once the loading of the original image is finished, the placeholder is replaced with the original image.

{% code title="src/app/app.component.ts" %}
```jsx
    ...
    transformation: Array<Transformation> = [{
        width: "400",
    }];
    lqip = { 
        active: true, quality: 20
    };
    ...
}
```
{% endcode %}

{% code title="src/app/app.component.html" %}
```jsx
<ik-image
    path={{path}}
    urlEndpoint="https://ik.imagekit.io/demo/"
    [transformation]="transformation"
    [lqip]="lqip"
    >
  </ik-image>
```
{% endcode %}

### **Combining lazy loading with low-quality placeholders**

You have the option to lazy-load the original image only when the user scrolls near them. Until then, only a low-quality placeholder is loaded. This saves a lot of network bandwidth if the user never scrolls further down.

{% code title="src/app/app.component.ts" %}
```jsx
    ...
    transformation: Array<Transformation> = [{
        height:"300",
        width: "400",
    }];
    lqip = { 
        active: true
    };
    ...
}
```
{% endcode %}

{% code title="src/app/app.component.html" %}
```jsx
// Loading a blurred low quality image placeholder 
// and lazy-loading original when the user scrolls near them
<ik-image
    path={{path}}
    urlEndpoint="https://ik.imagekit.io/demo/"
    [transformation]="transformation"
    [lqip]="lqip"
    loading="lazy"
    >
  </ik-image>
```
{% endcode %}

## **Uploading files in Angular**

Let's now learn how to upload an image to our media library.

Angular SDK provides `ik-upload` component, which renders an `input type="file"` tag that you can use to upload files to the [ImageKit media library](../../media-library/overview/) directly from the client-side.

To implement this functionality, a backend server is needed to authenticate the request using your [API private key](../../api-reference/api-introduction/api-keys.md#private-key).

### **Setup the backend app**

For this quickstart guide, we will create a sample Node.js server that will provide an authentication endpoint at `http://localhost:3000/auth`.

Let's a new folder, `server`, and create files `app.js` and `package.json` inside the new folder in the project root.

```bash
mkdir server
touch server/app.js
```

Package.json should look like:
```jsx
{
  "name": "server",
  "version": "1.0.0",
  "description": "Sample server for file upload using Imagekit SDK",
  "main": "app.js",
  "scripts": {
    "server": "nodemon app"
  },
  "devDependencies": {
    "cors": "^2.8.5",
    "crypto": "^1.0.1",
    "dotenv": "^8.2.0",
    "express": "^4.17.1",
    "nodemon": "^2.0.2",
    "router": "^1.3.3",
    "uuid": "^3.3.3"
  }
}
```

Now do `npm install`.

Next we setup the content for `app.js`.

{% tabs %}
{% tab title="app.js" %}
{% code title="server/app.js" %}
```javascript
const dotenv = require('dotenv');
const express = require('express');
const router = express.Router();
var cors = require('cors');
const app = express();
app.use(cors());

dotenv.config();

const uuid = require('uuid');
const crypto = require("crypto");

const privateKey = process.env.PRIVATE_KEY;

router.get("/auth", function(req, res) {
    var token = req.query.token || uuid.v4();
    var expire = req.query.expire || parseInt(Date.now()/1000)+2400;
    var privateAPIKey = `${privateKey}`;
    var signature = crypto.createHmac('sha1', privateAPIKey).update(token+expire).digest('hex');
    res.status(200);
    res.send({
        token : token,
        expire : expire,
        signature : signature
    });
});

app.use("/",router);

app.listen(3000,function(){
  console.log("Live at Port 3000");
});
```
{% endcode %}
{% endtab %}
{% endtabs %}

We would also need a .env file to indicate value for PRIVATE_KEY.
This can be taken from your imagekit account's setting page where PUBLIC_KEY can also be found.
Sample .env file should look like this.
```javascript
PRIVATE_KEY=<your-private-key>
```

Let's run the backend server.

```
cd server
npm run server
```

You should see a log saying that the app is _**“Live on port 3000”**_.

If you GET `http://localhost:3000/auth`, you should see a JSON response like this. Actual values will vary.

```javascript
{
    token: "5dd0e211-8d67-452e-9acd-954c0bd53a1f",
    expire: 1601047259,
    signature: "dcb8e72e2b6e98186ec56c62c9e62886f40eaa96"
}
```

### **Configure authentication in the frontend app**

Now that we have our authentication server up and running, let's configure the `publicKey` and `authenticator` in the frontend Angular app:

Add the following to `src/app/app.module.js` file to [initialize the SDK](angular.md#initialize-the-angular-sdk) with auth params:

```jsx
publicKey: "<YOUR_PUBLIC_KEY>",
```

### **Upload an image**

For this, we will use the `ik-upload` component as well as a couple of event handlers for upload error and success, `onError` and `onSuccess` respectively. Let's use it in our `app.component.html` file:

```jsx
<ik-upload 
    fileName="test.jpg" 
    (onError)="handleUploadError($event)"
    (onSuccess)="handleUploadSuccess($event)" 
    [authenticator]="authenticator"
></ik-upload>
```

In `app.component.js` file:

```jsx

// You can call a secure endpoint on your backend and pass any custom request headers for authentication purposes.

authenticator = async () => {
    try {

        // You can pass headers as well and later validate the request source in the backend, or you can use headers for any other use case.
        
        const headers = {
          'Authorization': 'Bearer your-access-token',
          'CustomHeader': 'CustomValue'
        };

        const response = await fetch('server_endpoint', {
            headers
        });

        if (!response.ok) {
            const errorText = await response.text();
            throw new Error(`Request failed with status ${response.status}: ${errorText}`);
        }

        const data = await response.json();
        const { signature, expire, token } = data;
        return { signature, expire, token };
    } catch (error) {
        throw new Error(`Authentication request failed: ${error.message}`);
    }
};
```

This is how it looks in the UI:

![Upload Image](<../../.gitbook/assets/angular/angular-sdk-file-upload-1.png>)

**Direct file uploading from the browser**

Let’s now upload an image by selecting a file from the file input.

When you choose a file, the file is immediately uploaded. You can pass optional `onSuccess` and `onError` callback functions as props like we have.

You can verify that file was successfully uploaded by checking the browser console. In case of success, it should print a success message, like this:

![Upload Success Response](<../../.gitbook/assets/angular/angular-sdk-file-upload-2.png>)

The response object would look similar to this (values may vary):

```jsx
{
    "fileId": "63a2e985e809dd54b027a563",
    "name": "test_RiCBw0ouh.jpg",
    "size": 36919,
    "versionInfo": {
        "id": "63a2e985e809dd54b027a563",
        "name": "Version 1"
    },
    "filePath": "/test_RiCBw0ouh.jpg",
    "url": "https://ik.imagekit.io/yzyzyz/test_RiCBw0ouh.jpg",
    "fileType": "image",
    "height": 500,
    "width": 500,
    "thumbnailUrl": "https://ik.imagekit.io/yzyzyz/tr:n-ik_ml_thumbnail/test_RiCBw0ouh.jpg",
    "AITags": null
}
```

After a successful upload, you should see the newly uploaded image in the [Media Library](https://imagekit.io/dashboard#media-library) section of your ImageKit dashboard.

If you don't see the image, check if there are any errors in the browser console log. Then verify whether the API [private key](../../api-reference/api-introduction/api-keys.md#private-key) has been configured correctly in the server app and if the server app is running.

**Fetching uploaded file**

Fetch the uploaded image and show in UI using `ik-image` with the `filePath` returned in the upload response.

```jsx
<ik-image path="/test_RiCBw0ouh.png" ></ik-image>
```

The app should display your uploaded image correctly!

## **Advanced file upload**

A more detailed example of how to use the file upload component (and an explanation of each advanced feature) is presented below:

{% code title="src/app/app.component.ts" %}
```jsx
import { Component } from '@angular/core';
import { Transformation } from 'imagekit-javascript/dist/src/interfaces/Transformation';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})

export class AppComponent {
  validateFileFunction(res: any) {
    console.log('validating')
    if(res.size < 1000000){ // Less than 1mb
      return true;
    }
    return false;
  }

  onUploadStartFunction(res: any) {
    console.log('onUploadStart')
  }

  onUploadProgressFunction(res: any) {
    console.log('progressing')
  }

  handleUploadSuccess(res: any) {
    console.log('File upload success with response: ', res);
    console.log(res.$ResponseMetadata.statusCode); // 200
    console.log(res.$ResponseMetadata.headers); // headers
    this.uploadedImageSource = res.url;
  }

  handleUploadError(err: any) {
    console.log('There was an error in upload: ', err);
    this.uploadErrorMessage = 'File upload failed.';
  }
}
```
{% endcode %}

### **Custom Upload Button**
The `buttonRef` property for `ik-upload` can be used so that the file selector component inside `ik-upload` will be invisible. This allows you to create your own `button`, with your own stylings, and reference it using `#<buttonRefValue>`. 

Here's an example:

```jsx
<div>
    <!-- This will be invisible -->
    <ik-upload
      [buttonRef]="myBtn"
    ></ik-upload>

    <!-- Your own button! -->
    <button #myBtn type="Button" class="myOwnClass" style="color:blue">
      Upload
    </button>
</div>
```

### **Abort Upload**

```js
// Added to app.component.ts
@ViewChild('upload') uploadComponent:IkUploadComponent;// @ViewChild can be used to get instance of IKUpload component.

onAbortFunction(){
    this.uploadComponent && this.uploadComponent.abort();
}

// Added to app.component.html
<ik-upload 
  #upload
  fileName= "test.jpg" 
  (onError)="handleUploadError($event)"
  (onSuccess)="handleUploadSuccess($event)"
  [validateFile]="validateFileFunction"
  [onUploadStart]="onUploadStartFunction"
  [onUploadProgress]="onUploadProgressFunction"
  [authenticator]="authenticator"
></ik-upload>
<button 
  (click)="onAbortFunction()"
>Abort</button>
```

### **Upload start**
The `onUploadStart` prop is called when the file upload starts. This can be used for common use cases like showing a spinner, progress bar, etc.

### **Show progress bar**
The `onUploadProgress` prop can be passed like above, which will have a [ProgressEvent](https://developer.mozilla.org/en-US/docs/Web/API/ProgressEvent). This can be used to show the percentage of upload progress to the end user.

### **Validate file before upload**
Arbitrary validation (file type, file size, file name) etc can be added using the `validateFile` prop. An example has been added above that shows how to prevent upload if the file size is bigger than 1 MB.

### **Additional options to the upload function**
All the parameters supported by the [ImageKit Upload API](https://docs.imagekit.io/api-reference/upload-file-api/client-side-file-upload) can be passed as shown above (e.g. `extensions`, `webhookUrl`, `customMetadata` etc.)

## **Rendering videos**

Rendering videos works similarly to rendering images in terms of usage of `urlEndpoint` param.

**Loading video from relative path:**
{% code title="src/app/app.component.html" %}
```jsx
<ik-video
    urlEndpoint="https://ik.imagekit.io/demo/"
    path="sample-video.mp4"
    controls=true
></ik-video>
```
{% endcode %}

## **What's next**

The possibilities for image manipulation and optimization with ImageKit are endless. Learn more about it here:

* [Image Transformations](https://docs.imagekit.io/features/image-transformations)
* [Image optimization](https://docs.imagekit.io/features/image-optimization)
* [Media Library](https://docs.imagekit.io/media-library/overview)
* [Performance monitoring](../../features/performance-monitoring.md)

