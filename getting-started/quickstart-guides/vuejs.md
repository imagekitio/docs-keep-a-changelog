---
description: >-
  Real-time image & video resizing, automatic optimization, and file uploading in Vue.js
  using ImageKit.io.
---

# Vue.js

This quick start guide shows you how to integrate ImageKit in the Vue.js application. The code samples covered here are hosted on GitHub -  [https://github.com/imagekit-samples/quickstart/tree/master/vuejs](https://github.com/imagekit-samples/quickstart/tree/master/vuejs).

This guide walks you through the following topics:

* [Setting up ImageKit Vue.js SDK](vuejs.md#setup-imagekit-vue.js-sdk)
* [Rendering images](vuejs.md#rendering-images-in-vue.js)
* [Setting the ImageKit context for the SDK](vuejs.md#setting-imagekit-context-for-the-sdk)
* [Applying common image manipulations](vuejs.md#common-image-manipulation-in-vue.js)
* [Adding overlays to images](vuejs.md#adding-overlays-to-images-in-vue.js)
* [Lazy loading images](vuejs.md#lazy-loading-images-in-vue.js)
* [Blurred image placeholder](vuejs.md#blurred-image-placeholder)
* [Client-side file uploading](vuejs.md#uploading-files-in-vue.js)
* [Rendering videos](vuejs.md#rendering-videos)

## Setup ImageKit Vue.js SDK

#### Install Vue CLI

```
npm install -g @vue/cli
# OR
yarn global add @vue/cli
```

#### Create a project

Let's create a dummy project called **vuejs** using vue CLI.

```
vue create vuejs
```

It will prompt the vue version and a few other options, select default by pressing enter. It will create a dummy project like this:

![](<../../.gitbook/assets/vue-initial-setup.png>)

#### Install imagekitio-vue&#x20;

```
npm install imagekitio-vue
# OR
yarn add imagekitio-vue
```

#### Initialize SDK

* `urlEndpoint` is the required parameter. You can get the value of URL-endpoint from your ImageKit dashboard - [https://imagekit.io/dashboard/url-endpoints](https://imagekit.io/dashboard/url-endpoints).
* `publicKey` and `authenticator` parameters are optional and only needed if you want to use the SDK for client-side file upload. `publicKey` can be obtained from the [Developer section](https://imagekit.io/dashboard/developer/api-keys) on your ImageKit dashboard.
* `authenticator` expects an asynchronous function that resolves with an object containing the necessary security parameters i.e signature, token, and expire.

Let's modify src/components/HelloWorld.vue to import and initialize ImageKit as a plugin. Replace `your_url_endpoint` etc, with actual values.

{% code title="src/components/HelloWorld.vue" %}
```javascript
<template>
  <div>
    <h1>ImageKit Vuejs quick start</h1>
  </div>
</template>

<script>
import { IKImage, IKContext, IKVideo, IKUpload } from "imagekitio-vue";

export default {
  name: "HelloWorld",
  components: {
    IKImage,
    IKContext,
    IKVideo,
    IKUpload,
  },
  props: {
    msg: String,
  }
}
</script>
```
{% endcode %}

This SDK provides 4 components:

* `IKImage` for [image resizing](https://github.com/imagekit-developer/imagekit-vuejs#image-resizing). The output is a `<img>` tag.
* `IKVideo` for [video resizing](#video-resizing). This renders a `<video>` tag.
* `IKUpload` for [file uploading](https://github.com/imagekit-developer/imagekit-vuejs#file-upload). The output is a `<input type="file">` tag.
* [`IKContext`](https://github.com/imagekit-developer/imagekit-vuejs#ik-context) for defining `urlEndpoint`, `publicKey` and `authenticator` to all children elements.

You can import components individually.

```javascript
import { IKImage, IKContext, IKVideo, IKUpload } from "imagekitio-vue";

export default {
  components: {
    IKImage,
    IKContext,
    IKVideo,
    IKUpload,
  }
}
```

## Rendering images in Vue.js

#### Loading image using a relative path

Let's render an image at path `/default-image.jpg`.

{% code title="src/components/HelloWorld.vue" %}
```javascript
<template>
  <div>
    <h1>ImageKit Vuejs quick start</h1>
    <IKImage :urlEndpoint="urlEndpoint" :path="path" width="400" />
  </div>
</template>

<script>
import { IKImage } from "imagekitio-vue";

let path = "/default-image.jpg";

export default {
  name: "HelloWorld",
  components: {
    IKImage,
  },
  data() {
    return {
      urlEndpoint: "https://ik.imagekit.io/demo",
      path,
    };
  },
  props: {
    msg: String,
  }
};
</script>
```
{% endcode %}

```javascript
<IKImage urlEndpoint="https://ik.imagekit.io/demo" path="/default-image.jpg" width="400" />
```

renders to:

```markup
<img src="https://ik.imagekit.io/demo/default-image.jpg?ik-sdk-version=vuejs-1.0.6" 
     class="ik-image"
     width="400">
```

The final result looks like this:

![](<../../.gitbook/assets/vue-image.png>)

#### Loading image from an absolute path

If you have an absolute image path coming from the backend API e.g. `https://www.custom-domain.com/default-image.jpg` then you can use `src` prop to load the image.

For example:

```javascript
<IKImage urlEndpoint="https://ik.imagekit.io/demo" width="400" src="https://ik.imagekit.io/demo/default-image.jpg" />
```

The output looks like this:

![](<../../.gitbook/assets/Screenshot 2020-09-25 at 5.52.41 PM.png>)

# **Setting ImageKit context for the SDK**

It is not necessary to specify the `urlEndpoint` in every instance of `IKImage`. This can be managed much more easily with the `IKContext` component.

`IKContext` is a wrapper that can be configured with your [SDK initialization parameters](vuejs.md#initialize-sdk). Pass your `urlEndpoint` to it as a prop, and you're good to go!

Let's go ahead and import it within the file:

```jsx
import { IKImage, IKContext } from "imagekitio-vue";
```

Now add the `IKContext` component to the render function:

```jsx
<IKContext urlEndpoint="your_url_endpoint"></IKContext>
```

Let's nest our `IKImage` components within it, so that those can access the `urlEndpoint` from the context wrapper.

```javascript
<template>
  <div>
    <IKContext :urlEndpoint="urlEndpoint">
      <IKImage path="default-image.jpg" width="400" />
    </IKContext>
  </div>
</template>
/* 
    Replace your_url_endpoint with actual values
*/
<script>

import { IKContext, IKImage } from "imagekitio-vue";

export default {
  name: "HelloWorld",
  components: {
    IKContext,
    IKImage,
  },
  data() {
    return {
      urlEndpoint: "your_url_endpoint",
    };
  },
  props: {
    msg: String,
  },
};
</script>
```

## Common image manipulation in Vue.js

This section covers the basics:

* [Resizing images ](vuejs.md#resizing-images-in-vue.js)
* [Quality manipulation](vuejs.md#quality-manipulation)
* [Chained transformation](vuejs.md#chained-transformation)

The Vuejs SDK gives a name to each transformation parameter e.g. `height` for `h` and `width` for `w` parameter. It makes your code more readable. If the property does not match any of the available options, it is added as it is. See the [full list of supported transformations](https://github.com/imagekit-developer/imagekit-vuejs#list-of-supported-transformations) in Vuejs SDK on Github.

:point\_right: Note that you can also use `h` and `w` parameter instead of `height` and `width`. See the complete list of transformations supported in ImageKit [here](../../features/image-transformations/resize-crop-and-other-transformations.md).

### Resizing images in Vue.js

Let's resize the image to width 300 and height 300.

```javascript
<IKImage 
  urlEndpoint="https://ik.imagekit.io/demo" 
  path="/default-image.jpg" 
  :transformation="[{ width: 300, height: 300 }]" />
```

The output looks like:

![](<../../.gitbook/assets/Screenshot 2020-09-25 at 5.52.29 PM.png>)

### Quality manipulation

You can use the [quality parameter](../../features/image-transformations/resize-crop-and-other-transformations.md#quality-q) to change quality like this:

```javascript
<IKImage 
  urlEndpoint="https://ik.imagekit.io/demo" 
  path="/default-image.jpg" 
  :transformation="[{quality: 40}]" />
```

The output is:

![](<../../.gitbook/assets/Screenshot 2020-09-25 at 6.12.00 PM.png>)

### Chained transformation

You can pass more than one object in `transformation` prop to chain these transformations sequentially.

For example, the following values will first resize the image to width 300, height 300, and then [rotate](../../features/image-transformations/resize-crop-and-other-transformations.md#rotate-rt) to 90 degrees.&#x20;

```javascript
// It means first resize the image to 400x400 and then rotate 90 degree
transformation = [
  {
    height: 300,
    width: 300
  },
  {
    rotation: 90
  }
]
```

So the following:

```javascript
<IKImage 
      urlEndpoint="https://ik.imagekit.io/demo"
      path="/default-image.jpg" 
      :transformation="[{width: 300, height: 300}, {rotation: 90}]" />
```

renders to:

![](<../../.gitbook/assets/Screenshot 2020-09-25 at 6.15.57 PM.png>)

## Adding overlays to images in Vue.js

ImageKit.io allows you to add [text](../../features/image-transformations/overlay-using-layers#add-text-over-image) and [image overlay](../../features/image-transformations/overlay-using-layers#transformation-of-image-overlay) dynamically.

For example:

```javascript
<IKImage 
  urlEndpoint="https://ik.imagekit.io/demo"
  path="/default-image.jpg" 
  :transformation="[{
    width: 300, 
    height: 300, 
    raw: 'l-image,i-default-image.jpg,w-100,b-10_CDDC39,l-end'
  }]" />
```

Renders to:

```javascript
<img class="ik-image" src="https://ik.imagekit.io/<YOUR_IMAGEKIT_ID>/tr:w-300,h-300,l-image,i-default-image.jpg,w-100,b-10_CDDC39,l-end/default-image.jpg">
```

The output looks like:

![](<../../.gitbook/assets/vuejs-sdk-overlay.png>)



## Lazy-loading images in Vue.js

You can lazy load images using the `loading` prop in `IKImage` component. When you use `loading="lazy"`, all images that are immediately viewable without scrolling load normally. Those that are far below the device viewport are only fetched when the user scrolls near them.

The SDK uses a fixed threshold based on the effective connection type to ensure that images are loaded early enough to finish loading once the user scrolls near them.

On fast connections (e.g. 4G), the value of the threshold is `1250px` and on slower connections (e.g. 3G), it is `2500px`.

{% hint style="info" %}
You should always set the `height` and `width` of image element to avoid [layout shift](https://www.youtube.com/watch?v=4-d\_SoCHeWE) when lazy-loading images.
{% endhint %}

Example usage:

```javascript
// Lazy loading images
<IKImage 
  urlEndpoint="https://ik.imagekit.io/demo"
  path="/default-image.jpg"
  :transformation="[{height:300,width:400}]"
  loading="lazy"
  height="300"
  width="400"
/>
```

## Blurred image placeholder

To improve user experience, you can use a low-quality blurred variant of the original image as a placeholder while the original image is being loaded in the background. Once the loading of the original image is finished, the placeholder is replaced with the original image.

```javascript
// Loading a blurred low quality image placeholder while the original image is being loaded
<IKImage 
  urlEndpoint="https://ik.imagekit.io/demo"
  path="/default-image.jpg"
  :lqip="{active:true}"
/>
```

By default, the SDK uses the `quality:20` and `blur:6`. You can change this. For example:

```javascript
<IKImage 
  urlEndpoint="https://ik.imagekit.io/demo"
  path="/default-image.jpg"
  :lqip="{active:true, quality: 40, blur: 5}"
/>
```

#### Combining lazy loading with low-quality placeholders

You have the option to lazy-load the original image only when the user scrolls near them. Until then, only a low-quality placeholder is loaded. This saves a lot of network bandwidth if the user never scrolls further down.

```javascript
// Loading a blurred low quality image placeholder and lazy-loading original when user scrolls near them
<IKImage 
  urlEndpoint="https://ik.imagekit.io/demo"
  path="/default-image.jpg"
  :transformation="[{height:300,width:400}]"
  :lqip="{active:true}"
  loading="lazy"
  height="300"
  width="400"
/>
```

## Uploading files in Vue.js

Vuejs SDK provides an `IKUpload` component, which can generate an `input type="file"` tag that you can use to upload files to the [ImageKit media library](../../media-library/overview/) directly from the client-side.&#x20;

For using upload functionality, we need to pass `publicKey` and `authenticator` in [`IKContext`](https://github.com/imagekit-developer/imagekit-vuejs#ik-context).  Replace `your_url_endpoint` , `your_public_key`, `your_authentication_endpoint` with actual values.

```javascript
  <IKContext publicKey="your_public_key" urlEndpoint="your_url_endpoint" :authenticator="authenticator">
    <IKUpload
      :onError="onError"
      :onSuccess="onSuccess"
    />
  </IKContext>
```

> Make sure that you have specified `authenticator` and `publicKey` in the parent IKContext component as a prop. The authenticator expects an asynchronous function that resolves with an object containing the necessary security parameters i.e signature, token, and expire. Endpoint for `authenticator` should be implemented in your backend. [Learn how to implement endpoint for authenticator](https://docs.imagekit.io/api-reference/upload-file-api/client-side-file-upload#how-to-implement-authenticationendpoint-endpoint) on your server.

It is advised to setup a backend server for the creation of these security parameters. In the frontend an HTTP GET request can be made to fetch them using the `authenticator` function.

For this quickstart guide, we have provided a sample implementation of `http://localhost:3001/auth` in Node.js.

Let's create a file `index.js` inside `server` folder in the project root. It should look like this:

![](<../../.gitbook/assets/Screenshot 2020-09-25 at 6.46.25 PM.png>)

We will use the [ImageKit Node.js SDK](https://github.com/imagekit-developer/imagekit-nodejs) to implement `http://localhost:3001/auth`.

Let's first install all basic modules we need to run an HTTP server.

```bash
npm install express
npm install imagekit
```

Let's modify index.js to implement `http://localhost:3001/auth` which is our `authenticationEndpoint`.

{% tabs %}
{% tab title="index.js (backend)" %}
{% code title="server/index.js" %}
```javascript
/* 
    This is our backend server.
    Replace your_url_endpoint, your_public_key, your_private_key 
    with actual values
*/
const express = require('express');
const app = express();
const ImageKit = require("imagekit");

const imagekit = new ImageKit({
    urlEndpoint: "your_url_endpoint",
    publicKey: "your_public_key",
    privateKey: "your_privage_key"
})

app.get("/auth", function (req, res) {
    var result = imagekit.getAuthenticationParameters();
    res.send(result);
});

app.listen(3001, function () {
    console.log("Live at Port 3001");
});
```
{% endcode %}
{% endtab %}

{% tab title="HelloWorld.vue (frontend)" %}
{% code title="src/components/HelloWorld.vue" %}
```javascript
<script>

import { IKContext, IKUpload } from "imagekitio-vue";

export default {
  name: "HelloWorld",
  components: {
    IKContext,
    IKUpload,
  },
  data() {
    return {
      urlEndpoint: "your_url_endpoint",
      publicKey: "your_public_key",
    };
  },
  props: {
    msg: String,
  },
  methods: {
    authenticator() {
      return new Promise((resolve, reject) => {
        var url = "http://localhost:3001/auth";

        // Make the Fetch API request
        fetch(url, { method: "GET", mode: "cors" }) // Enable CORS mode
          .then((response) => {
            if (!response.ok) {
              throw new Error(`HTTP error! Status: ${response.status}`);
            }
            return response.json();
          })
          .then((body) => {
            var obj = {
              signature: body.signature,
              expire: body.expire,
              token: body.token,
            };
            resolve(obj);
          })
          .catch((error) => {
            reject([error]);
          });
      });
    },
  },
};
</script>
```
{% endcode %}
{% endtab %}
{% endtabs %}

Let's run the backend server.

```
cd server
node index.js
```

If you GET http://localhost:3001/auth, you should see a JSON response like this. Actual values will vary.

```javascript
{
    token: "5dd0e211-8d67-452e-9acd-954c0bd53a1f",
    expire: 1601047259,
    signature: "dcb8e72e2b6e98186ec56c62c9e62886f40eaa96"
}
```

Let's include `IKUpload` component in the `HelloWorld.vue`.

```javascript
<template>
  <div>
    <h1>ImageKit Vuejs quick start</h1>

    <h2>File upload</h2>
    <IKContext :publicKey="publicKey" :urlEndpoint="urlEndpoint" :authenticator="authenticator">
      <IKUpload
        :onError="onError"
        :onSuccess="onSuccess"
        :onUploadProgress="onUploadProgress"
        :onUploadStart="onUploadStart"
        :validateFile="validateFile" 
      />
    </IKContext>
  </div>
</template>
/* 
    Replace your_url_endpoint, your_public_key with actual values
*/
<script>

import { IKContext, IKUpload } from "imagekitio-vue";

export default {
  name: "HelloWorld",
  components: {
    IKContext,
    IKUpload,
  },
  data() {
    return {
      urlEndpoint: "your_url_endpoint",
      publicKey: "your_public_key",
    };
  },
  props: {
    msg: String,
  },
  methods: {
    onError(err) {
      try {
        console.log("Error");
        console.log(err);
      } catch (e) {
        console.error(e);
      }
    },
    onSuccess(res) {
      try {
        console.log("Success");
        console.log(res);
      } catch (e) {
        console.error(e);
      }
    },
    validateFile(res) {
      if (res.size > 0) {
        return true;
      }
      return false;
    },
    onUploadStart(event) {
      console.log("Upload started");
      console.log(event);
    },
    onUploadProgress(event) {
      console.log(`Upload inprogress ... (${((event.loaded * 100) / event.total).toFixed(2)}% Done)`);
      console.log(event);
    },
    authenticator() {
      return new Promise((resolve, reject) => {
        var url = "http://localhost:3001/auth";

        // Make the Fetch API request
        fetch(url, { method: "GET", mode: "cors" }) // Enable CORS mode
          .then((response) => {
            if (!response.ok) {
              throw new Error(`HTTP error! Status: ${response.status}`);
            }
            return response.json();
          })
          .then((body) => {
            var obj = {
              signature: body.signature,
              expire: body.expire,
              token: body.token,
            };
            resolve(obj);
          })
          .catch((error) => {
            reject([error]);
          });
      });
    },
  },
};
</script>
```

The output looks like:

![](<../../.gitbook/assets/Screenshot 2020-09-25 at 8.55.09 PM.png>)

When you choose a file, the file is uploaded. You can pass optional `onSuccess`, `onUploadStart`, `onUploadProgress`,  `validateFile` and `onError` callback functions as props like we have.

After successful upload, you should see the upload API response in the console like this:

![](<../../.gitbook/assets/Screenshot 2020-09-25 at 8.57.39 PM.png>)

#### Abort upload

`ref` can be passed to obtain access to the IKUpload component's instance. Calling the `triggerAbortUpload` method will abort the upload if any is in progress.

Example Usage

```js
<template>
  <IKUpload 
    ref="childComponentRef" 
    :publicKey="publicKey" 
    :urlEndpoint="urlEndpoint"
    :authenticator="authenticator"
    :tags="['tag1','tag2']"
    :responseFields="['tags']"
    :onError="onError"
    :onSuccess="onSuccess"
    customCoordinates="10,10,100,100"
  />
  <button @click="abortChildUpload">Abort Child Upload</button>
</template>

<script>
import { IKUpload } from "imagekitio-vue"

export default {
  name: "app",
  components: {},
  data() {
    return {};
  },
  methods: {
    onError(err) {
      console.log("Error");
      console.log(err);
    },
    onSuccess(res) {
      console.log("Success");
      console.log(res);
    }
    abortChildUpload() {
      this.$refs.childComponentRef.triggerAbortUpload();
      console.log("Upload aborted")
    },
  }
};
</script>
```

## **Rendering videos**

Rendering videos works similarly to rendering images in terms of usage of `urlEndpoint` param (either directly or via `IKContext`).

**Loading video from relative path:**
Import `IKVideo` from the SDK:

```jsx
import { IKVideo} from "imagekitio-vue";

export default {
  components: {
    IKVideo
  }
}
```

Now let's add it to our App. Along with the video path prop, it also needs the relevant `urlEndpoint` (either directly or via `IKContext`):
```jsx
<IKContext urlEndpoint="https://ik.imagekit.io/demo">
  <IKVideo
    :path="videoPath"
    :transformation="[{ height: 200, width: 200 }]"
    :controls="true"
  />
</IKContext>
```

A more complex example:
```jsx
<IKContext urlEndpoint="https://ik.imagekit.io/demo">
  <IKVideo
    className='ikvideo-with-tr'
    :path="videoPath"
    :transformation="[{ height: 200, width: 600, b: '5_red', q: 95 }]"
    :controls="true"
  />
</IKContext>
```

## **What's next**

The possibilities for image manipulation and optimization with ImageKit are endless. Learn more about it here:&#x20;

* [Image Transformations](https://docs.imagekit.io/features/image-transformations)
* [Image optimization](https://docs.imagekit.io/features/image-optimization)
* [Media Library](https://docs.imagekit.io/media-library/overview)
* [Performance monitoring](../../features/performance-monitoring.md)
