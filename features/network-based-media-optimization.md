# Network-based media optimization

{% hint style="warning" %}
**Note:** The service worker offered by ImageKit for network-based image optimization is now deprecated and will not receive any updates or fixes from ImageKit. If you are using the same on your website and need any support, reach out to us at support@imagekit.io, or refer to the general technique below to implement network-based media optimization on your own.
{% endhint %}

With ImageKit.io, you can build the functionality to deliver different images and videos based on the end user's network connection speed. For example, a user experiencing slow download times on a 2G network can be served a lighter, more compressed media file, while users on a 4G network can be served higher quality files. This ensures that you deliver the best user experience to all your users regardless of their bandwidth constraints.

Let's look at how you can implement network-based media optimization on your websites and apps.

## How to build apps and websites with network-based media optimization

There are two steps to building this:

### 1. Determine the user's network speed
There are different ways of doing this on apps and websites. 

On any browser, web or mobile, that supports the `NetworkInformation` API, you can get the approximate category of the download speeds of the user using the value of `navigator.connection.effectiveType`. 
There are more accurate ways of determining the user's effective download speed on apps. Refer to these answers to get hints on how to do this 
1. [For Android Apps](https://stackoverflow.com/a/55161717)
2. [For iOS Apps](https://stackoverflow.com/a/9496235)

### 2. Adjust the file size based on the network speed
Once you have determined whether the user has a fast or slow download speed, you can decide the compression level for that user. Remember that to decrease the file size, reduce the value of the quality transformation parameter in the URL. 

1. [Quality transformation for images](../features/image-optimization/quality-optimization.md)
2. [Quality transformation for videos](../features/video-optimization/quality-optimization.md)

For example, to a user with a fast download speed, you can serve the file at the default compression level (without the quality transformation in the URL)

```
https://ik.imagekit.io/demo/default-image.jpg?tr=w-300
```

And to a user experiencing slow network speeds, you can reduce the quality to 50 in the URL

```
https://ik.imagekit.io/demo/default-image.jpg?tr=w-300,q-50
```

Reducing the quality parameter, however, negatively impacts visual quality. So ensure that you don't take it too low. For images, keeping the quality parameter over 40 will work fine in most cases, while for videos keeping it over 30 will work fine. Test the lower quality threshold for your files and see if it suits your business.


**Where to make this quality parameter change in your app or website**
1. For apps, since the layout is rendered on the client-side, you can change the media URLs you receive from your backend APIs and add the quality parameter to them before adding them to the layout.

2. For web and mobile web, if you use a client-side rendering framework like React or Vue, then you can use the same technique as point 1 - add the quality parameter to the file URL you receive from the backend and use it on your website. 
   
3. If you use a server-side rendered website, the server won't know the user's effective connection type. In such cases, you will need to intercept the file requests in a service worker, modify their URL with the quality parameter, and get the response from the modified URL. We have written a sample service worker that does the same and also uses separate network-based client-side caches for files . You can [find the same here](https://github.com/imagekit-developer/network-based-image-optimization/blob/95e934ee868eae7eb2913af7ed5d4e211d0a4ce9/sw_v2.js).

### Points to consider
1. You may not want to optimize all the files based on the user's network. For example, your logo or a sale banner should always appear in high quality on the app or website. Take this into account when implementing network-based media optimization.
   
2. Find the quality thresholds that work for your business for different network types. Some businesses may be unable to increase the compression as it reduces the visual quality.
   
3. See if you can do away with network-based optimization by performing other format, quality, and responsive image optimizations or by reducing the default compression level. It will save you a lot of development time while giving you a good user experience for all your users. Use standard tools such as [Google Lighthouse or PageSpeed](https://imagekit.io/blog/improve-google-pagespeed-insights-score-for-images/) to see if your media files are completing all the basic checks for an optimized experience.