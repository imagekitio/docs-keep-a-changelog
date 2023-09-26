---
description: >-
  Real-time image resizing, automatic optimization, and file uploading in React
  using ImageKit.io.
---

# React

This is a quick start guide to show you how to integrate ImageKit in a React application. The code samples covered here are hosted on GitHub: [https://github.com/imagekit-samples/quickstart/tree/master/react](https://github.com/imagekit-samples/quickstart/tree/master/react).

This guide walks you through the following topics: ‌

* [Setting up ImageKit React SDK](react.md#setup-imagekit-react-sdk)
* [Rendering images](react.md#rendering-images)
* [Setting the ImageKit context for the SDK](react.md#setting-authentication-context-for-the-sdk)
* [Applying common image manipulations](react.md#basic-image-manipulation)
* [Adding overlays to images](react.md#adding-overlays-to-images)
* [Lazy loading images](react.md#lazy-loading-images-in-react)
* [Blurred image placeholder](react.md#blurred-image-placeholder)
* [Client-side file uploading](react.md#uploading-files-in-react)
* [Advanced file uploading](react.md#advanced-file-upload)
* [Rendering videos](react.md#rendering-videos)

## **Setup ImageKit React SDK**

For this tutorial, it is recommended to create a dummy React app, as shown below.&#x20;

**Create a React app:**

Let's use the `create-react-app` CLI utility provided by React to build a new project:

```bash
npx create-react-app imagekit-react-app
```

Navigate to the project directory:

```
cd imagekit-react-app/
```

Open up the project in your text editor of choice, and navigate to `src/App.js`. This is where we will do most of our work. It should look like this:

{% code title="src/App.js" %}
```jsx
import React from 'react';
import logo from './logo.svg';
import './App.css';

function App() {
  return (
    <div className="App">
      <header className="App-header">
        <img src={logo} className="App-logo" alt="logo" />
        <p>
          Edit <code>src/App.js</code> and save to reload.
        </p>
        <a
          className="App-link"
          href="https://reactjs.org"
          target="_blank"
          rel="noopener noreferrer"
        >
          Learn React
        </a>
      </header>
    </div>
  );
}

export default App;
```
{% endcode %}

&#x20;Now run the app:

```
npm start
```

In your web browser, navigate to `http://localhost:3000/`

You should see the dummy app created by React CLI. Now we can begin our work.

**Install the ImageKit React SDK:**

Installing the ImageKit React SDK in our app is pretty simple:

```
npm install --save imagekitio-react
```

#### **Initialize the React SDK:**

Before the SDK can be used, let's learn about and obtain the requisite initialization parameters:

* `urlEndpoint` is a required parameter. This can be obtained from the [URL-endpoint section](https://imagekit.io/dashboard#url-endpoints) or the [developer section](https://imagekit.io/dashboard#developers) on your ImageKit dashboard.
* `publicKey` and `authenticator` parameters are optional and only needed if you want to use the SDK for client-side file upload. `publicKey` can be obtained from the [Developer section](https://imagekit.io/dashboard#developers) on your ImageKit dashboard.
* `authenticator` expects an asynchronous function that resolves with an object containing the necessary security parameters i.e signature, token, and expire.

```javascript
// required parameter to fetch images
const urlEndpoint = '<YOUR_IMAGEKIT_URL_ENDPOINT>';

// optional parameters (needed for client-side upload)
const publicKey = '<YOUR_IMAGEKIT_PUBLIC_KEY>'; 
const authenticator = ()=>{
  return new Promise((resolve,reject)=>{
    resolve({signature,token,expiry})
  })
};
```

{% hint style="info" %}
_**Note:**_ _Do not include your _[_API private key_](../../api-reference/api-introduction/api-keys.md#private-key)_ in any client-side code, including this SDK or its initialization. If you pass the `privateKey` parameter while initializing this SDK, it will throw an error._
{% endhint %}

**ImageKit Components:**

This SDK provides 3 components:

* `IKImage` for [image rendering](https://github.com/imagekit-developer/imagekit-react#ikimage---url-generation). The output is a `<img>` tag.
* `IKUpload` for [file uploading](https://github.com/imagekit-developer/imagekit-react#ikupload---file-upload). The output is a `<input type="file">` tag.
* `IKContext` for defining [authentication context](https://github.com/imagekit-developer/imagekit-react#ikcontext), i.e. `urlEndpoint`, `publicKey` and `authenticator` to all child elements.

You can import components individually:

```javascript
import { IKImage, IKContext, IKUpload } from 'imagekitio-react';
```

#### Configure the app for ImageKit:

Let's remove the existing dummy code in `src/App.js` file, then add the `urlEndpoint`:

{% code title="src/App.js" %}
```jsx
import React from 'react';
import './App.css';

const urlEndpoint = '<YOUR_IMAGEKIT_URL_ENDPOINT>';

function App() {
  return (
    <div className="App">
    </div>
  );
}

export default App;
```
{% endcode %}

## **Rendering images**

**Loading image from relative path:**

Remember the default image we mentioned earlier? It should be available at the following URL:

```
https://ik.imagekit.io/<YOUR_IMAGEKIT_ID>/default-image.jpg
```

Let's fetch and display it! For this, we will use the `IKImage` component.&#x20;

Import `IKImage` from the SDK:

```jsx
import { IKImage } from 'imagekitio-react';
```

Now let's add it to our `App`. Along with the image `path` prop, it also needs the prop for `urlEndpoint`:

```jsx
<IKImage
  urlEndpoint={urlEndpoint}
  path="default-image.jpg"
/>
```

**Rendered HTML element:**

```jsx
<img src="https://ik.imagekit.io/demo/default-image.jpg?ik-sdk-version=react-1.x.x" alt="">
```

The `App.js` file should look like this now:

{% code title="src/App.js" %}
```jsx
import React from 'react';
import './App.css';
import { IKImage } from 'imagekitio-react';

const urlEndpoint = '<YOUR_IMAGEKIT_URL_ENDPOINT>';

function App() {
  return (
    <div className="App">
      <h1>ImageKit React quick start</h1>
      <IKImage
        urlEndpoint={urlEndpoint} 
        path="default-image.jpg"
      />
    </div>
  );
}

export default App;
```
{% endcode %}

Your React app should now display the default image in its full size:

![Full sized image (1000px x 1000 px)](<../../.gitbook/assets/react-sdk-render-image (1).png>)

You can pass styles and other attributes as props. For e.g. lets add 400px width_ _by adding the `width` prop:

```jsx
<IKImage
  urlEndpoint={urlEndpoint}
  path="default-image.jpg"
  width="400"
/>
```

**This is how the output should look now:**

![Resized image (width=400px)](<../../.gitbook/assets/react-sdk-render-image-resized (1).png>)

{% hint style="info" %}
Note that here we have set the width to 400px at the `<img>` tag level only. Intrinsically, the fetched image is still 1000px wide.
{% endhint %}

**Loading image from an absolute path:**

If you have an absolute image path coming from the backend API e.g. `https://www.custom-domain.com/default-image.jpg` then you can use `src` prop to load the image.

**For example:**

```jsx
<IKImage
  urlEndpoint={urlEndpoint}
  src="https://ik.imagekit.io/demo/default-image.jpg"
  width="400"
/>

```

**The output looks like this:**

![Render image on custom domain via absolute path](<../../.gitbook/assets/react-sdk-render-image-absolute-path (1).png>)

## **Setting ImageKit context for the SDK**

It is not necessary to specify the `urlEndpoint` in every instance of `IKImage`. This can be managed much more easily with the `IKContext` component.

`IKContext` is a wrapper that can be configured with your [SDK initialization parameters](react.md#initialize-the-react-sdk). Pass your `urlEndpoint` to it as a prop, and you're good to go!

Let's go ahead and import it within the `App.js` file:

```jsx
import { IKContext, IKImage } from 'imagekitio-react';
```

Now add the `IKContext` component to the render function:

```jsx
<IKContext urlEndpoint={urlEndpoint}></IKContext>
```

Let's nest our `IKImage` components within it, so that those can access the `urlEndpoint` from the context wrapper.

{% code title="src/App.js" %}
```jsx
import React from 'react';
import './App.css';
import { IKContext, IKImage } from 'imagekitio-react';

const urlEndpoint = '<YOUR_IMAGEKIT_URL_ENDPOINT>';

function App() {
  return (
    <div className="App">
      <IKContext urlEndpoint={urlEndpoint}>
        <h1>ImageKit React quick start</h1>
        <IKImage path="default-image.jpg" width="400" />
        
        <h2>Loading image from an absolute path</h2>
        <IKImage
          src="https://ik.imagekit.io/demo/default-image.jpg"
          width="400"
        />
      </IKContext>
    </div>
  );
}

export default App;
```
{% endcode %}

## **Basic image manipulation**

Let’s now learn how to manipulate images using transformations.

* [Resizing images](react.md#height-and-width-manipulation)&#x20;
* [Quality manipulation](react.md#quality-manipulation)
* [Crop mode](react.md#crop-mode)
* [Chained transformation](react.md#chained-transformation)

The React SDK gives a name to each transformation parameter, e.g. `height` for `h` and `width` for `w` parameter. It makes your code more readable. If the property does not match any of the available options, it is added as it is. See the [full list of supported transformations](https://github.com/imagekit-developer/imagekit-react#list-of-supported-transformations) in React SDK on Github.

{% hint style="info" %}
You can also use `h` and `w` parameter instead of `height` and `width`.\
See the complete list of transformations supported in ImageKit [here](../../features/image-transformations/resize-crop-and-other-transformations.md).
{% endhint %}

### **Height and width manipulation**

‌To resize an image along with its [height](../../features/image-transformations/resize-crop-and-other-transformations.md#height-h) or [width](../../features/image-transformations/resize-crop-and-other-transformations.md#width-w), we need to pass the `transformation` object as a prop to `IKImage`.

Let’s resize the default image to 200px height and width:

```jsx
<IKImage
  path="default-image.jpg"
  transformation={[{
    height: 200,
    width: 200
  }]}
/>
```

**Rendered HTML element:**

```markup
<img src="https://ik.imagekit.io/<YOUR_IMAGEKIT_ID>/tr:h-200,w-200/default-image.jpg?ik-sdk-version=react-1.x.x" 
  path="default-image.jpg" 
  alt="">
```

Refresh your browser to get the resized image.

![Resized Image (200x200px)](<../../.gitbook/assets/react-sdk-basic-height-width (1).png>)

### **Quality manipulation**

You can use the [quality parameter](../../features/image-transformations/resize-crop-and-other-transformations.md#quality-q) to change image quality like this**:**

```jsx
<IKImage
  path="default-image.jpg"
  transformation={[{ quality: 10 }]}
  width="400"
/>
```

**Rendered HTML:**

```jsx
<img src="https://ik.imagekit.io/<YOUR_IMAGEKIT_ID>/tr:q-10/default-image.jpg?ik-sdk-version=react-1.x.x" 
  path="default-image.jpg" width="400" alt="">
```

![Quality manipulation (q=10)](<../../.gitbook/assets/react-sdk-basic-quality (1).png>)

### **Crop mode**‌

Let’s now see how [cropping](../../features/image-transformations/resize-crop-and-other-transformations.md#crop-crop-modes-and-focus) works. We will try the [`extract`](../../features/image-transformations/resize-crop-and-other-transformations.md#extract-crop-strategy-cm-extract) crop strategy.  In this strategy, instead of resizing the whole image, we extract out a region of the requested dimension from the original image. You can read more about this [here](https://docs.imagekit.io/features/image-transformations/resize-crop-and-other-transformations#extract-crop-strategy-cm-extract).&#x20;

```jsx
<IKImage
  path="default-image.jpg"
  transformation={[{
    height: 300,
    width: 200,
    cropMode: 'extract',
  }]}
/>
```

**Rendered HTML element:**

```markup
<img src="https://ik.imagekit.io/<YOUR_IMAGEKIT_ID>/tr:h-300,w-200,cm-extract/default-image.jpg" 
  path="default-image.jpg" 
  alt="">
```

![Crop Mode Extract (200x300px)](<../../.gitbook/assets/react-sdk-basic-crop-mode (1).png>)

### **Chained transformation**

[Chained transformations](https://docs.imagekit.io/features/image-transformations/chained-transformations) provide a simple way to control the sequence in which transformations are applied.

Let’s try it out by [resizing](../../features/image-transformations/resize-crop-and-other-transformations.md#basic-image-resizing) an image, then [rotating](../../features/image-transformations/resize-crop-and-other-transformations.md#rotate-rt) it:

```jsx
<IKImage
  path="default-image.jpg"
  transformation={[{
    height: 300,
    width: 200
  }]}
/>
```

**Transformation URL:**

```markup
<img src="https://ik.imagekit.io/<YOUR_IMAGEKIT_ID>/tr:h-300,w-200/default-image.jpg" 
  path="default-image.jpg" 
  alt="">
```

![Resized and cropped (200x300px)](<../../.gitbook/assets/react-sdk-basic-chained-1 (1).png>)

Now, rotate the image by 90 degrees.

```jsx
<IKImage
  path="default-image.jpg"
  transformation={[{
    height: 300,
    width: 200,
  }, { 
    rt: 90,
  }]}
/>
```

**Chained Transformation URL:**

```markup
<img src="https://ik.imagekit.io/<YOUR_IMAGEKIT_ID>/tr:h-300,w-200:rt-90/default-image.jpg" 
  path="default-image.jpg" 
  alt="">
```

![Resized, then rotated](<../../.gitbook/assets/react-sdk-basic-chained-2 (1).png>)

Let’s flip the order of transformation and see what happens.

```jsx
<IKImage
  path="default-image.jpg"
  transformation={[{
    rt: 90,
  }, { 
    height: 300,
    width: 200,
  }]}
/>
```

**Chained Transformation URL:**

```markup
<img src="https://ik.imagekit.io/<YOUR_IMAGEKIT_ID>/tr:rt-90:h-300,w-200/default-image.jpg" 
  path="default-image.jpg" 
  alt="">
```

![Rotated, then resized](<../../.gitbook/assets/react-sdk-basic-chained-3 (1).png>)

## **Adding overlays to images**

ImageKit.io allows you to add [text](../../features/image-transformations/overlay.md#text-overlay) and [image overlay](../../features/image-transformations/overlay.md) dynamically.

For example, a text overlay can be used to superimpose text on an image. Try it like so:

```jsx
<IKImage
  path="default-image.jpg"
  transformation={[{
    height: 300,
    width: 300,
    overlayText: 'ImageKit',
    overlayTextFontSize: 50,
    overlayTextColor: '0651D5',
  }]}
/>
```

**Rendered HTML element:**

```markup
<img src="https://ik.imagekit.io/<YOUR_IMAGEKIT_ID>/tr:h-300,w-300,ot-ImageKit,ots-50,otc-0651D5/default-image.jpg" 
  path="default-image.jpg" 
  alt="">
```

![Text Overlay (300x300px)](<../../.gitbook/assets/react-sdk-overlay-text (1).png>)

## **Lazy-loading images in React**

You can lazy load images using the `loading` prop in `IKImage` component. When you use `loading="lazy"`, all images that are immediately viewable without scrolling load normally. Those that are far below the device viewport are only fetched when the user scrolls near them.

The SDK uses a fixed threshold based on the effective connection type to ensure that images are loaded early enough so that they have finished loading once the user scrolls near to them.

{% hint style="info" %}
You should always set the height and width of the image element to avoid[ layout shift](https://www.youtube.com/watch?v=4-d\_SoCHeWE) when lazy-loading images.
{% endhint %}

```jsx
<IKImage
  path="default-image.jpg"
  transformation={[{ height: 300, width: 400 }]}
  loading="lazy"
  height="300"
  width="400"
/>
```

**Rendered HTML element:**

```jsx
<img src="https://ik.imagekit.io/<YOUR_IMAGEKIT_ID>/tr:h-300,w-400/default-image.jpg?ik-sdk-version=react-1.x.x" 
  path="default-image.jpg" loading="lazy" height="300" width="400" alt="">
```

## **Blurred image placeholder**

To improve user experience, you can use a low-quality [blurred](../../features/image-transformations/resize-crop-and-other-transformations.md#blur-bl) variant of the original image as a placeholder while the original image is being loaded in the background. Once the loading of the original image is finished, the placeholder is replaced with the original image.

```jsx
// Loading a blurred low quality image placeholder 
// while the original image is being loaded
<IKImage
  path="default-image.jpg"
  lqip={{ active: true, quality: 20 }}
  width="400"
/>
```

### **Combining lazy loading with low-quality placeholders**

You have the option to lazy-load the original image only when the user scrolls near them. Until then, only a low-quality placeholder is loaded. This saves a lot of network bandwidth if the user never scrolls further down.

```jsx
// Loading a blurred low quality image placeholder 
// and lazy-loading original when user scrolls near them
<IKImage
  path="default-image.jpg"
  transformation={[{ height:300, width:400 }]}
  lqip={{ active:true }}
  loading="lazy"
  height="300"
  width="400"
/>
```

## **Uploading files in React**

Let's now learn how to upload an image to our media library.

React SDK provides `IKUpload` component which renders an `input type="file"` tag that you can use to upload files to the [ImageKit media library](../../media-library/overview/) directly from the client-side.

To implement this functionality, a backend server is needed to authenticate the request using your [API private key](../../api-reference/api-introduction/api-keys.md#private-key).

### **Setup the backend app**

For this quickstart guide, we will create a sample Node.js server which will provide an authentication endpoint at `http://localhost:3001/auth`.&#x20;

Let's create a file `index.js` inside `server` folder in the project root.

```bash
mkdir server
touch server/index.js
```

Install the basic packages needed to create a dummy server for ImageKit backend authentication:

```jsx
npm install --save express imagekit
```

We will use the[ ImageKit Node.js SDK](https://github.com/imagekit-developer/imagekit-nodejs) to implement `http://localhost:3001/auth`.

The backend SDK requires your API [public key](../../api-reference/api-introduction/api-keys.md#public-key), [private key](../../api-reference/api-introduction/api-keys.md#private-key), and [URL endpoint](../../integration/url-endpoints.md). You can obtain them from [Developer Options](https://imagekit.io/dashboard/developer/api-keys) and [URL-endpoint](https://imagekit.io/dashboard/url-endpoints) pages respectively.

This is how `server/index.js` file should look now. Replace `<YOUR_IMAGEKIT_URL_ENDPOINT>`, `<YOUR_IMAGEKIT_PUBLIC_KEY>` and `<YOUR_IMAGEKIT_PRIVATE_KEY>` with actual values:

{% tabs %}
{% tab title="Node.js" %}
{% code title="server/index.js" %}
```javascript
/* 
  This is our backend server.
  Replace YOUR_IMAGEKIT_URL_ENDPOINT, YOUR_IMAGEKIT_PUBLIC_KEY, 
  and YOUR_IMAGEKIT_PRIVATE_KEY with actual values
*/
const express = require('express');
const app = express();
const ImageKit = require('imagekit');

const imagekit = new ImageKit({
  urlEndpoint: '<YOUR_IMAGEKIT_URL_ENDPOINT>',
  publicKey: '<YOUR_IMAGEKIT_PUBLIC_KEY>',
  privateKey: '<YOUR_IMAGEKIT_PRIVATE_KEY>'
});

// allow cross-origin requests
app.use(function(req, res, next) {
  res.header("Access-Control-Allow-Origin", "*");
  res.header("Access-Control-Allow-Headers", 
    "Origin, X-Requested-With, Content-Type, Accept");
  next();
});

app.get('/auth', function (req, res) {
  var result = imagekit.getAuthenticationParameters();
  res.send(result);
});

app.listen(3001, function () {
  console.log('Live at Port 3001');
});
```
{% endcode %}
{% endtab %}
{% endtabs %}

Let's run the backend server.

```
cd server
node index.js
```

You should see a log saying that the app is _**“Live on port 3001”**_.

If you GET `http://localhost:3001/auth`, you should see a JSON response like this. Actual values will vary.

```javascript
{
    token: "5dd0e211-8d67-452e-9acd-954c0bd53a1f",
    expire: 1601047259,
    signature: "dcb8e72e2b6e98186ec56c62c9e62886f40eaa96"
}
```

### **Configure authentication in the frontend app**

Now that we have our authentication server up and running, let's configure the `publicKey` and `authenticator` in the frontend React app:

Add the following to `src/App.js` file to [initialize the SDK](react.md#initialize-the-react-sdk) with auth params:

```jsx
const publicKey = '<YOUR_IMAGEKIT_PUBLIC_KEY>'; 
const authenticator =  async () => {
    try {
        const response = await fetch('http://localhost:3001/auth');

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

Now, pass these values as props into a new `IKContext` instance which will hold our upload component:

```jsx
<IKContext publicKey={publicKey} urlEndpoint={urlEndpoint} authenticator={authenticator} >
  {/* ...child components */}
</IKContext>
```

This is how `src/App.js` should look now. Replace `<YOUR_IMAGEKIT_URL_ENDPOINT>` and `<YOUR_IMAGEKIT_PUBLIC_KEY>` with actual values:

{% code title="src/App.js" %}
```jsx
import React from 'react';
import './App.css';
import { IKContext, IKImage } from 'imagekitio-react';

const urlEndpoint = '<YOUR_IMAGEKIT_URL_ENDPOINT>';
const publicKey = '<YOUR_IMAGEKIT_PUBLIC_KEY>'; 
const authenticator =  async () => {
    try {
        const response = await fetch('http://localhost:3001/auth');

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

function App() {
  return (
    <div className="App">
      <IKContext
        urlEndpoint={urlEndpoint}
        publicKey={publicKey}
        authenticator={authenticator}
      >
        {/* ...client side upload component goes here */}
      </IKContext>
      {/* ...other SDK components added previously */}
    </div>
  );
}

export default App;
```
{% endcode %}

### **Upload an image**

For this, we will use the `IKUpload` component. Let's import it from the SDK into our `App.js` file:

```jsx
import { IKContext, IKImage, IKUpload } from 'imagekitio-react';
```

Add the `IKUpload` component nested within `IKContext`, as well as a couple of event handlers for upload error and success, `onError` and `onSuccess` respectively:

{% tabs %}
{% tab title="React JSX" %}
{% code title="src/App.js" %}
```jsx
import React from 'react';
import './App.css';
import { IKContext, IKImage, IKUpload } from 'imagekitio-react';

const publicKey = '<YOUR_IMAGEKIT_PUBLIC_KEY>';
const urlEndpoint = '<YOUR_IMAGEKIT_URL_ENDPOINT>';
const authenticator =  async () => {
    try {
        const response = await fetch('http://localhost:3001/auth');

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

const onError = err => {
  console.log("Error", err);
};

const onSuccess = res => {
  console.log("Success", res);
};

function App() {
  return (
    <div className="App">
      <h1>ImageKit React quick start</h1>
      <IKContext 
        publicKey={publicKey} 
        urlEndpoint={urlEndpoint} 
        authenticator={authenticator} 
      >
        <p>Upload an image</p>
        <IKUpload
          fileName="test-upload.png"
          onError={onError}
          onSuccess={onSuccess}
        />
      </IKContext>
      {/* ...other SDK components added previously */}
    </div>
  );
}

export default App;
```
{% endcode %}
{% endtab %}
{% endtabs %}

This is how it looks in the UI:

![Upload Image](<../../.gitbook/assets/react-sdk-file-upload-1 (1).png>)

**Direct file uploading from the browser**

Let’s now upload an image by selecting a file from the file input.&#x20;

When you choose a file, the file is immediately uploaded. You can pass optional `onSuccess` and `onError` callback functions as props like we have.

You can verify that file was successfully uploaded by checking the browser console. In case of success, it should print a success message, like this:

![Upload Success Response](<../../.gitbook/assets/react-sdk-file-upload-2 (2).png>)

The response object would look similar to this (values may vary):

```jsx
{
  fileId: "5f7d39aa80da9a54b1d7976a",
  filePath: "/test-upload_0GfSgMm4p.png",
  fileType: "image",
  height: 298,
  name: "test-upload_0GfSgMm4p.png",
  size: 5748,
  thumbnailUrl: "https://ik.imagekit.io/<YOUR_IMAGEKIT_ID>/tr:n-media_library_thumbnail/test-upload_0GfSgMm4p.png",
  url: "https://ik.imagekit.io/<YOUR_IMAGEKIT_ID>/test-upload_0GfSgMm4p.png",
  width: 298,
}
```

After a successful upload, you should see the newly uploaded image in the [Media Library](https://imagekit.io/dashboard#media-library) section of your ImageKit dashboard.

If you don't see the image, check if there are any errors in the browser console log. Then verify whether the API [private key](../../api-reference/api-introduction/api-keys.md#private-key) has been configured correctly in the server app and if the server app is running.

**Fetching uploaded file**

Fetch uploaded image and show in UI using `IKImage` with the `filePath` returned in the upload response.

```jsx
<IKImage path="/test-upload_0GfSgMm4p.png" />
```

The app should display your uploaded image correctly!

## **Advanced file upload**

A more detailed example of how to use the file upload component (and an explanation of each advanced feature) is presented below:

{% tabs %}
{% tab title="React JSX" %}
{% code title="src/App.js" %}
```jsx
import React, { useRef } from 'react';
import './App.css';
import { IKContext, IKImage, IKUpload } from 'imagekitio-react';

const publicKey = '<YOUR_IMAGEKIT_PUBLIC_KEY>';
const urlEndpoint = '<YOUR_IMAGEKIT_URL_ENDPOINT>';
const authenticationEndpoint = 'http://localhost:3001/auth';

const onError = err => {
  console.log("Error", err);
};

const onSuccess = res => {
  console.log("Success", res);
};

const onUploadProgress = progress => {
  console.log("Progress", progress);
};

const onUploadStart = evt => {
  console.log("Start", evt);
};

function App() {
  const inputRefTest = useRef(null);
  const ikUploadRefTest = useRef(null);
  return (
    <div className="App">
      <h1>ImageKit React quick start</h1>
      <IKContext 
        publicKey={publicKey} 
        urlEndpoint={urlEndpoint} 
        authenticationEndpoint={authenticationEndpoint} 
      >
        <p>Upload an image with advanced options</p>
        <IKUpload
          fileName="test-upload.jpg"
          tags={["sample-tag1", "sample-tag2"]}
          customCoordinates={"10,10,10,10"}
          isPrivateFile={false}
          useUniqueFileName={true}
          responseFields={["tags"]}
          validateFile={file => file.size < 2000000}
          folder={"/sample-folder"}
          extensions={[{
            "name": "remove-bg",
            "options": {
              "add_shadow": true,
            },
          }]}
          webhookUrl="https://www.example.com/imagekit-webhook" // replace with your webhookUrl
          overwriteFile={true}
          overwriteAITags={true}
          overwriteTags={true}
          overwriteCustomMetadata={true}
          // customMetadata={{
          //   "brand": "Nike",
          //   "color": "red",
          // }}
          onError={onError}
          onSuccess={onSuccess}
          onUploadProgress={onUploadProgress}
          onUploadStart={onUploadStart}
          // style={{display: 'none'}} // hide the default input and use the custom upload button
          inputRef={inputRefTest}
          ref={ikUploadRefTest}
        />
        <p>Custom Upload Button</p>
        {inputRefTest && <button onClick={() => inputRefTest.current.click()}>Upload</button>}
        <p>Abort upload request</p>
        {ikUploadRefTest && <button onClick={() => ikUploadRefTest.current.abort()}>Abort request</button>}
      </IKContext>
      {/* ...other SDK components added previously */}
    </div>
  );
}

export default App;
```
{% endcode %}
{% endtab %}
{% endtabs %}

### **Custom Upload Button**
We have created a `ref` to the `input` used inside the `IKUpload` component called `inputRefTest`. The `IKUpload` component can be given styling via `className` or `style` (`style={{display: 'none'}}`) to hide the default file selector. Then we can use the custom upload button as described above.

### **Abort uploads**
We have created a `ref` to the `IKUpload` component called `ikUploadRefTest`. This `ref` can be used to call the `abort` method in the `IKUpload` component and can be used to abort the ongoing upload.

### **Upload start**
The `onUploadStart` prop is called when the file upload starts. This can be used for common use cases like showing a spinner, progress bar, etc.

### **Show progress bar**
The `onUploadProgress` prop can be passed like above, which will have a [ProgressEvent](https://developer.mozilla.org/en-US/docs/Web/API/ProgressEvent). This can be used to show the percentage of upload progress to the end user.

### **Validate file before upload**
Arbitrary validation (file type, file size, file name) etc can be added using the `validateFile` prop. An example has been added above that shows how to prevent upload if the file size is bigger than 2 MB.

### **Additional options to the upload function**
All the parameters supported by the [ImageKit Upload API](https://docs.imagekit.io/api-reference/upload-file-api/client-side-file-upload) can be passed as shown above (e.g. `extensions`, `webhookUrl`, `customMetadata` etc.)

## **Rendering videos**

Rendering videos works similarly to rendering images in terms of usage of `urlEndpoint` param (either directly or via `IKContext`).

**Loading video from relative path:**
Import `IKVideo` from the SDK:

```jsx
import { IKVideo } from 'imagekitio-react';
```

Now let's add it to our App. Along with the video path prop, it also needs the relevant `urlEndpoint` (either directly or via `IKContext`):
```jsx
<IKContext urlEndpoint={<YOUR_IMAGEKIT_URL_ENDPOINT>}>
  <IKVideo
    className='ikvideo-default'
    path={videoPath}
    transformation={[{ height: 200, width: 200 }]}
    controls={true}
  />
</IKContext>
```

A more complex example:
```jsx
<IKContext urlEndpoint={<YOUR_IMAGEKIT_URL_ENDPOINT>}>
  <IKVideo
    className='ikvideo-with-tr'
    path={videoPath}
    transformation={[{ height: 200, width: 600, b: '5_red', q: 95 }]}
    controls={true}
  />
</IKContext>
```

## **Error boundaries**

We strongly recommend using [Error Boundaries](https://reactjs.org/docs/error-boundaries.html) to handle errors in the React UI. `ErrorBoundary` is used to gracefully handle errors anywhere in the child component tree of a React app.&#x20;

It can be used to log errors and display a fallback UI instead of the component tree that crashed.

#### **Example:**

```jsx
<ErrorBoundary>
  <IKImage publicKey={publicKey} urlEndpoint={urlEndpoint} path={path} 
    transformation={[{
      height: 300,
      width: 400
    }]} 
  />
</ErrorBoundary>
```

## **What's next**

The possibilities for image manipulation and optimization with ImageKit are endless. Learn more about it here:&#x20;

* [Image Transformations](https://docs.imagekit.io/features/image-transformations)
* [Image optimization](https://docs.imagekit.io/features/image-optimization)
* [Media Library](https://docs.imagekit.io/media-library/overview)
* [Performance monitoring](../../features/performance-monitoring.md)

