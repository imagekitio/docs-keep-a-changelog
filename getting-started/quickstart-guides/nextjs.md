---
description: >-
  Real-time image & video resizing, automatic optimization, and file uploading in Next.js using ImageKit.io.
---

# Next.js

This quick start guide demonstrates how to integrate ImageKit into a React application. The code samples provided are available on GitHub: [https://github.com/imagekit-samples/quickstart/tree/master/next](https://github.com/imagekit-samples/quickstart/tree/master/next).

This guide covers the following topics:

* [Setting up ImageKit Next.js SDK](nextjs.md#setup-imagekit-next.js-sdk)
* [Rendering images](nextjs.md#rendering-images)
* [Setting the ImageKit context for the SDK](nextjs.md#setting-imagekit-context-for-the-sdk)
* [Applying common image manipulations](nextjs.md#basic-image-manipulation)
* [Adding overlays](nextjs.md#adding-overlays)
* [Lazy loading images](nextjs.md#lazy-loading-images-in-next.js)
* [Blurred image placeholder](nextjs.md#blurred-image-placeholder)
* [Client-side file uploading](nextjs.md#uploading-files-in-next.js)
* [Advanced file uploading](nextjs.md#advanced-file-upload)
* [Rendering videos](nextjs.md#rendering-videos)

## **Setup ImageKit Next.js SDK**

For this tutorial, it is recommended to create a dummy Next.js app, as demonstrated below.

**Create a Next.js app:**

Let's use the `create-next-app` CLI utility provided by Next.js to build a new project:

```bash
npx create-next-app@latest imagekit-next-app
```

We will be using below congiguration for the dummy app
```js
✔ Would you like to use TypeScript?  No
✔ Would you like to use ESLint? Yes
✔ Would you like to use Tailwind CSS? No 
✔ Would you like to use `src/` directory? No
✔ Would you like to use App Router? (recommended) Yes
✔ Would you like to customize the default import alias (@/*)? No
```

Navigate to the project directory:

```
cd imagekit-next-app/
```

Open up the project in your text editor of choice, and navigate to `app/page.js`. This is where we will do most of our work. It should look like this:

{% code title="app/page.js" %}
```jsx
import Image from "next/image";
import styles from "./page.module.css";

export default function Home() {
  return (
    <main className={styles.main}>
      <div className={styles.description}>
        <p>
          Get started by editing&nbsp;
          <code className={styles.code}>app/page.js</code>
        </p>
        <div>
          <a
            href="https://vercel.com?utm_source=create-next-app&utm_medium=appdir-template&utm_campaign=create-next-app"
            target="_blank"
            rel="noopener noreferrer"
          >
            By{" "}
            <Image
              src="/vercel.svg"
              alt="Vercel Logo"
              className={styles.vercelLogo}
              width={100}
              height={24}
              priority
            />
          </a>
        </div>
      </div>

      <div className={styles.center}>
        <Image
          className={styles.logo}
          src="/next.svg"
          alt="Next.js Logo"
          width={180}
          height={37}
          priority
        />
      </div>

      ...
      
    </main>
  );
}
```
{% endcode %}

&#x20;Now run the app:

```
npm run dev
```

In your web browser, navigate to `http://localhost:3000/`

You should see the dummy app created by React CLI. Now we can begin our work. It should appear like this.

![Resized and cropped (200x300px)](<../../.gitbook/assets/next/initial-next.png>)


**Install the ImageKit Next.js SDK:**

Installing the ImageKit Next.js SDK in our app is pretty simple:

```
npm install --save imagekit-next
```

#### **Initialize the Next SDK:**

Before the SDK can be used, let's learn about and obtain the requisite initialization parameters:

* `urlEndpoint` is a required parameter. This can be obtained from the [URL-endpoint section](https://imagekit.io/dashboard/url-endpoints) or the [developer section](https://imagekit.io/dashboard/developer/api-keys) on your ImageKit dashboard.
* `publicKey` and `authenticator` parameters are optional and only needed if you want to use the SDK for client-side file upload. `publicKey` can be obtained from the [Developer section](https://imagekit.io/dashboard/developer/api-keys) on your ImageKit dashboard.
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
* `IKVideo` for [video resizing](https://github.com/imagekit-developer/imagekit-react#video-resizing). This renders a `<video>` tag.
* `IKUpload` for [file uploading](https://github.com/imagekit-developer/imagekit-react#file-upload). The output is a `<input type="file">` tag.
* `IKContext` for defining [authentication context](https://github.com/imagekit-developer/imagekit-react#ikcontext), i.e. `urlEndpoint`, `publicKey` and `authenticator` to all child elements.

You can import components individually:

```javascript
import { IKImage, IKVideo, IKContext, IKUpload } from 'imagekit-next';
```

#### Configure the app for ImageKit:

Let's remove the existing dummy code in `app/page.js` file, then add the `urlEndpoint`:

{% code title="app/page.js" %}
```jsx
import React from "react";

const urlEndpoint = "<YOUR_IMAGEKIT_URL_ENDPOINT>";

export default function Home() {
  return <div></div>;
}

```
{% endcode %}
















Image resizing can be used automatically with Next.js' [next/image component](https://nextjs.org/docs/api-reference/next/image). 

Here is an implementation using a [custom loader](https://nextjs.org/docs/api-reference/next/image#loader) which applies ImageKit image resizing, next/image will set an optimal width and quality for a given client.

Inside the `imageKitLoader` function, you will have to replace `urlEndpoint` with your ImageKit account's URL-endpoint. You can get URL-endpoint from your ImageKit dashboard - https://imagekit.io/dashboard/url-endpoints.

```javascript
import Image from "next/image";

const imageKitLoader = ({ src, width, quality }) => {
  if(src[0] === "/") src = src.slice(1);
  const params = [`w-${width}`];
  if (quality) {
    params.push(`q-${quality}`);
  }
  const paramsString = params.join(",");
  var urlEndpoint = "https://ik.imagekit.io/your_imagekit_id";
  if(urlEndpoint[urlEndpoint.length-1] === "/") urlEndpoint = urlEndpoint.substring(0, urlEndpoint.length - 1);
  return `${urlEndpoint}/${src}?tr=${paramsString}`
}

const MyImage = (props) => {
  return (
    <Image
      loader={imageKitLoader}
      src="default-image.jpg"
      alt="Sample image"
      width={400}
      height={400}
    />
  );
};
```