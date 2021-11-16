---
description: >-
  Learn how to troubleshoot 400 and 404 errors while accessing the image through
  ImageKit.io
---

# Error troubleshooting

When ImageKit gets an error while trying to fetch the original image from your external origin, it responds with `404` status code along with a generic "Not Found" message like below:

![](<../.gitbook/assets/Screenshot 2020-10-05 at 7.36.53 PM.png>)

To help you troubleshot the error, the response is accompanied by an** **`ik-error`** **header, which expounds more detail about the nature of the error.&#x20;

The value of `ik-error` header is: `ERROR_CODE - ERROR_MESSAGE`

For example, when you try to access an image from your AWS S3 bucket that doesn't exist, you will receive a `404` status code with an `ik-error`_ _header as follows:

`ENOENT - No such file was found on S3 at the specified path`

You can use browser developer tools to inspect the response header and read the error message. For example, if we try to access the below non-existing image:

```
http://ik.imagekit.io/violetviolinist/false/RANDOM_NAME.jpg
```

ImageKit returns a blank white page with a `Not Found` message, but you can inspect the `ik-error` header from the developer tools (Ctrl + Shift + i for Chrome) like this

![ik-error has a value composed of an error code and an error message](<../.gitbook/assets/image (14).png>)

This helps you identify that the error is caused because the file name specified in the URL does not exist on the origin. The origin, in this case, is an S3 bucket, but it can be [any origin](../integration/configure-origin/) that ImageKit supports.

Similarly, the header may consist of one of the other possible error codes and an accompanying message:

| Error Code   | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| ------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ENOENT       | It means the file does not exist at the given path on the origin server or object storage. Check the origin server associated with the URL-endpoint and ensure that the file exists.                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| EACCES       | Invalid or expired authentication credentials for the corresponding origin. In this case, you should check the credentials provided in the ImageKit dashboard while configuring origin and ensure that ImageKit.io has the necessary permission to access the file. In the case of [web server origin](../integration/configure-origin/web-server-origin.md), you should check firewall settings and [whitelist our servers](../integration/configure-origin/web-server-origin.md#whitelist-request-from-imagekit-io). In the case of object storage, you should ensure that the configured credential has read access. |
| ELIMIT       | Refer to [limits](limits.md) to understand more about the reason for the error. The response status is `400` in this case.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| ECONNREFUSED | The external origin refused to connect. This usually happens if your origin web server has some maximum connection limit, or a firewall has blocked our request. You should check firewall settings and [whitelist our servers](../integration/configure-origin/web-server-origin.md#whitelist-request-from-imagekit-io).                                                                                                                                                                                                                                                                                               |
| UNIDENTIFIED | The exact nature of the error could not be identified. This is rare, in this case, contact us at support@imagekit.io.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
