---
description: >-
  Real-time image resizing and automatic optimization in Next.js using ImageKit.io.
---

# Next.js

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