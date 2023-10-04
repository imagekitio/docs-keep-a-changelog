---
description: >-
  Real-time image resizing, automatic optimization, and file uploading in Vue.js
  using ImageKit.io.
---

# Vue.js

This is a quick start guide to show you how to integrate ImageKit in the Vue.js application. The code samples covered here are hosted on Github -  [https://github.com/imagekit-samples/quickstart/tree/master/vuejs](https://github.com/imagekit-samples/quickstart/tree/master/vuejs).

This guide walks you through the following topics:

* [Setting up ImageKit Vue.js SDK](vuejs.md#setup-imagekit-vue-js-sdk)
* [Rendering images](vuejs.md#rendering-images-in-vue-js)
* [Applying common image manipulations](vuejs.md#common-image-manipulation-in-vue-js)
* [Adding overlays to images](vuejs.md#adding-overlays-to-images-in-vue-js)
* [Lazy loading images](vuejs.md#lazy-loading-images-in-vue-js)
* [Blurred image placeholder](vuejs.md#blurred-image-placeholder)
* [Client-side file uploading](vuejs.md#uploading-files-in-vue-js)

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

![](<../../.gitbook/assets/Screenshot 2020-09-25 at 1.34.44 PM.png>)

#### Install imagekitio-vue&#x20;

```
npm install imagekitio-vue
# OR
yarn add imagekitio-vue
```

#### Initialize SDK

```javascript
import ImageKit from "imagekitio-vue"
Vue.use(ImageKit, {
  urlEndpoint: "your_url_endpoint",
  publicKey: "your_public_api_key",
})
```

* `urlEndpoint` is the required parameter. You can get the value of URL-endpoint from your ImageKit dashboard - [https://imagekit.io/dashboard/url-endpoints](https://imagekit.io/dashboard/url-endpoints).
* `publicKey` and `authenticator` parameters are optional and only needed if you want to use the SDK for client-side file upload. You can get `publicKey` parameters from the developer section in your ImageKit dashboard - [https://imagekit.io/dashboard/developer/api-keys](https://imagekit.io/dashboard/developer/api-keys).
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
import Vue from "vue"
import ImageKit from "imagekitio-vue"
Vue.use(ImageKit, {
  urlEndpoint: "your_url_endpoint", // Required. Default URL-endpoint is https://ik.imagekit.io/your_imagekit_id
  publicKey: "your_public_api_key", // optional
})

export default {
  name: 'HelloWorld',
  props: {
    msg: String
  }
}
</script>
```
{% endcode %}

This SDK provides 3 global components when registered as a plugin:

* `ik-image` for [image resizing](https://github.com/imagekit-developer/imagekit-vuejs#image-resizing). The output is a `<img>` tag.
* `ik-upload` for [file uploading](https://github.com/imagekit-developer/imagekit-vuejs#file-upload). The output is a `<input type="file">` tag.
* [`ik-context`](https://github.com/imagekit-developer/imagekit-vuejs#ik-context) for defining `urlEndpoint`, `publicKey` and `authenticator` to all children elements.

You can also import components individually.

```javascript
import { IKImage, IKContext, IKUpload } from "imagekitio-vue"

export default {
  components: {
    IKImage,
    IKContext,
    IKUpload
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
    <ik-image width="400" path="/default-image.jpg">
    </ik-image>
  </div>
</template>

<script>
import Vue from "vue"
import ImageKit from "imagekitio-vue"
Vue.use(ImageKit, {
  urlEndpoint: "https://ik.imagekit.io/demo"
})

export default {
  name: 'HelloWorld',
  props: {
    msg: String
  }
}
</script>
```
{% endcode %}

```javascript
<ik-image width="400" path="/default-image.jpg">
```

renders to:

```markup
<img src="https://ik.imagekit.io/demo/default-image.jpg?ik-sdk-version=vuejs-1.0.6" 
     class="ik-image"
     width="400">
```

The final result looks like this:

![](<../../.gitbook/assets/Screenshot 2020-09-25 at 4.04.40 PM.png>)

#### Loading image from an absolute path

If you have an absolute image path coming from the backend API e.g. `https://www.custom-domain.com/default-image.jpg` then you can use `src` prop to load the image.

For example:

```javascript
<ik-image 
    width="400" 
    src="https://ik.imagekit.io/demo/default-image.jpg" />
```

The output looks like this:

![](<../../.gitbook/assets/Screenshot 2020-09-25 at 5.52.41 PM.png>)

## Common image manipulation in Vue.js

This section covers the basics:

* [Resizing images ](vuejs.md#resizing-images-in-vue-js)
* [Quality manipulation](vuejs.md#quality-manipulation)
* [Chained transformation](vuejs.md#chained-transformation)

The Vuejs SDK gives a name to each transformation parameter e.g. `height` for `h` and `width` for `w` parameter. It makes your code more readable. If the property does not match any of the available options, it is added as it is. See the [full list of supported transformations](https://github.com/imagekit-developer/imagekit-vuejs#list-of-supported-transformations) in Vuejs SDK on Github.

:point\_right: Note that you can also use `h` and `w` parameter instead of `height` and `width`. See the complete list of transformations supported in ImageKit [here](../../features/image-transformations/resize-crop-and-other-transformations.md).

### Resizing images in Vue.js

Let's resize the image to width 300 and height 300.

```javascript
<ik-image 
      path="/default-image.jpg" 
      :transformation="[{width: 300, height: 300}]" />
```

The output looks like:

![](<../../.gitbook/assets/Screenshot 2020-09-25 at 5.52.29 PM.png>)

### Quality manipulation

You can use the [quality parameter](../../features/image-transformations/resize-crop-and-other-transformations.md#quality-q) to change quality like this:

```javascript
<ik-image 
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
<ik-image 
      path="/default-image.jpg" 
      :transformation="[{width: 300, height: 300}, {rotation: 90}]" />
```

renders to:

![](<../../.gitbook/assets/Screenshot 2020-09-25 at 6.15.57 PM.png>)

## Adding overlays to images in Vue.js

ImageKit.io allows you to can add [text](../../features/image-transformations/overlay.md#text-overlay) and [image overlay](../../features/image-transformations/overlay.md) dynamically.

For example:

```javascript
<ik-image 
  path="/default-image.jpg" 
  :transformation="[{
    width: 300, 
    height: 300, 
    overlayImage: 'default-image.jpg', 
    overlayWidth: 100,
    overlayX: 0,
    overlayImageBorder: '10_CDDC39' // 10px border of color CDDC39
  }]" />
```

Renders to:

```javascript
<img src="https://ik.imagekit.io/pshbwfiho/tr:w-300,h-300,oi-default-image.jpg,ow-100,ox-0,oib-10_CDDC39/default-image.jpg?ik-sdk-version=vuejs-1.0.8" class="ik-image">
```

The output looks like:

![](<../../.gitbook/assets/Screenshot 2020-09-26 at 2.51.34 PM.png>)



## Lazy-loading images in Vue.js

You can lazy load images using the `loading` prop in `ik-image` component. When you use `loading="lazy"`, all images that are immediately viewable without scrolling load normally. Those that are far below the device viewport are only fetched when the user scrolls near them.

The SDK uses a fixed threshold based on the effective connection type to ensure that images are loaded early enough so that they have finished loading once the user scrolls near to them.

On fast connections (e.g 4G), the value of the threshold is `1250px` and on slower connections (e.g 3G), it is `2500px`.

{% hint style="info" %}
You should always set the `height` and `width` of image element to avoid [layout shift](https://www.youtube.com/watch?v=4-d\_SoCHeWE) when lazy-loading images.
{% endhint %}

Example usage:

```javascript
// Lazy loading images
<ik-image
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
<ik-image
  path="/default-image.jpg"
  :lqip="{active:true}"
/>
```

By default, the SDK uses the `quality:20` and `blur:6`. You can change this. For example:

```javascript
<ik-image
  path="/default-image.jpg"
  :lqip="{active:true, quality: 40, blur: 5}"
/>
```

#### Combining lazy loading with low-quality placeholders

You have the option to lazy-load the original image only when the user scrolls near them. Until then, only a low-quality placeholder is loaded. This saves a lot of network bandwidth if the user never scrolls further down.

```javascript
// Loading a blurred low quality image placeholder and lazy-loading original when user scrolls near them
<ik-image
  path="/default-image.jpg"
  :transformation="[{height:300,width:400}]"
  :lqip="{active:true}"
  loading="lazy"
  height="300"
  width="400"
/>
```

## Uploading files in Vue.js

Vuejs SDK provides `ik-upload` component which can generate an `input type="file"` tag that you can use to upload files to the [ImageKit media library](../../media-library/overview/) directly from the client-side.&#x20;

For using upload functionality, we need to pass `publicKey` while [initializing the SDK](vuejs.md#initialize-sdk).  Replace `your_url_endpoint` and `your_public_key` with actual values.

```javascript
<script>
import Vue from "vue";
import ImageKit from "imagekitio-vue";
Vue.use(ImageKit, {
  urlEndpoint: "your_url_endpoint",
  publicKey: "your_public_key",
});

export default {
  name: "HelloWorld",
  props: {
    msg: String
  }
};
</script>
```
The SDK internally requires the security parameters as an object with three fields i.e. `signature`, `token`, and `expire`.
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
/* 
    This is our frontend code
    Replace your_url_endpoint, your_public_key with actual values
*/
import Vue from "vue";
import ImageKit from "imagekitio-vue";
Vue.use(ImageKit, {
  urlEndpoint: "your_url_endpoint",
  publicKey: "your_public_key",
});

export default {
  name: "HelloWorld",
  props: {
    msg: String
  }
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

Let's include `ik-upload` component in the `HelloWorld.vue`.

The component utilises an asynchronous function named `authenticator`, which is intended to be used for retrieving security parameters from your backend. This function is expected to resolve an object containing three fields: `signature`, `token`, and `expire`.

```javascript
<template>
  <div>
    <h1>ImageKit Vuejs quick start</h1>

    <h2>File upload</h2>
    <ik-upload 
      :onError="onError"
      :onSuccess="onSuccess" 
      :authenticator="authenticator"/>
  </div>
</template>
<script>
/* 
    Replace your_url_endpoint, your_public_key with actual values
*/
import Vue from "vue";
import ImageKit from "imagekitio-vue";
Vue.use(ImageKit, {
  urlEndpoint: "your_url_endpoint",
  publicKey: "your_public_key",
});

export default {
  name: "HelloWorld",
  props: {
    msg: String
  },
  methods: {
    authenticator =  async () => {
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
    },
    onError(err) {
      console.log("Error");
      console.log(err);
    },
    onSuccess(res) {
      console.log("Success");
      console.log(res);
    }
  }
};
</script>
```

The output looks like:

![](<../../.gitbook/assets/Screenshot 2020-09-25 at 8.55.09 PM.png>)

When you choose a file, the file is uploaded. You can pass optional `onSuccess` and `onError` callback functions as props like we have.

After successful upload, you should see the upload API response in the console like this:

![](<../../.gitbook/assets/Screenshot 2020-09-25 at 8.57.39 PM.png>)

## **What's next**

The possibilities for image manipulation and optimization with ImageKit are endless. Learn more about it here:&#x20;

* [Image Transformations](https://docs.imagekit.io/features/image-transformations)
* [Image optimization](https://docs.imagekit.io/features/image-optimization)
* [Media Library](https://docs.imagekit.io/media-library/overview)
* [Performance monitoring](../../features/performance-monitoring.md)
