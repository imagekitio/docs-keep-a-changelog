---
description: >-
  Give your images an SEO-friendly name without modifying the actual file name
  in the storage
---

# SEO-friendly image URL

### ImageKit helps you create SEO-friendly URLs dynamically

Image SEO starts with the file name. Google uses the URL path as well as the file name to understand your images.

For example, consider the following image of the Eiffel Tower:

![](../.gitbook/assets/eiffel-tower.jpg)

The image URL should have "Eiffel Tower" instead of "DSC1234.jpg". 

{% hint style="danger" %}
**Bad image URL**  
https://ik.imagekit.io/demo/DSC1234.jpg
{% endhint %}

{% hint style="success" %}
**SEO-friendly image URL**  
https://ik.imagekit.io/demo/eiffel-tower.jpg
{% endhint %}

### Dynamic SEO suffix \(ik-seo\)

When you cannot modify the file names of already stored images, ImageKit helps you create SEO-friendly URLs dynamically.

For example, let say you have the following image of the Eiffel Tower.

```text
https://ik.imagekit.io/demo/DSC1234.jpg
```

Here, `https://ik.imagekit.io/demo/` is your [URL-endpoint](../integration/url-endpoints.md).

You can dynamically use **eiffel-tower.jpg** as the file name using `ik-seo` parameter. For example:

```text
https://ik.imagekit.io/demo/ik-seo/DSC1234/eiffel-tower.jpg
```

So the following URL:  
  
https://ik.imagekit.io/demo/`ik-seo`/DSC1234/eiffel-tower.jpg 

will fetch the same image as:  
  
https://ik.imagekit.io/demo/DSC1234.jpg

Essentially

your-url-endpoint/old-file-name.extension 

becomes:

your\_url\_endpoint/**`ik-seo`**/old-file-name/**`seo-friendly-file-name`**.extension

### Accessing file that is stored in a nested folder

If your file is stored inside a nested folder e.g.

`https://ik.imagekit.io/demo/path/of/folder/old-file-name.jpg`

You can still dynamically add an SEO-friendly suffix like this:

`https://ik.imagekit.io/demo/ik-seo/path/of/folder/old-file-name/seo-friendly-file-name.jpg`

## Examples

Let's say we have the following URL:

`https://ik.imagekit.io/your_imagekit_id/default-image.jpg`

We want to change the file name from `default-image.jpg` to `seo-friendly-file-name.jpg`

So the new URL becomes

`https://ik.imagekit.io/your_imagekit_id/ik-seo/default-image/seo-friendly-file-name.jpg`

Let's do this using the [client-side SDKs](../api-reference/api-introduction/sdk.md#client-side-sdks):

{% tabs %}
{% tab title="Javascript" %}
```javascript
// Without ik-seo
var imageURL = imagekit.url({
    path: "/default-image.jpg",
    urlEndpoint: "https://ik.imagekit.io/your_imagekit_id/",
    transformation: [{
        height: 300,
        width: 400
    }]
});

// With ik-seo
var imageURL = imagekit.url({
    path: "/default-image/seo-friendly-file-name.jpg",
    urlEndpoint: "https://ik.imagekit.io/your_imagekit_id/ik-seo",
    transformation: [{
        height: 300,
        width: 400
    }]
});
```
{% endtab %}

{% tab title="React" %}
```javascript
// Without ik-seo, urlEndpoint has been defined in parent IKContext 
<IKImage
  path="/default-image.jpg"
  transformation={[{
    height: 300,
    width: 400
  }]}
/>
  
// With ik-seo
<IKImage
  urlEndpoint="https://ik.imagekit.io/your_imagekit_id/ik-seo"
  path="/default-image/seo-friendly-file-name.jpg"
  transformation={[{
    height: 300,
    width: 400
  }]}
/>
```
{% endtab %}

{% tab title="Vue.js" %}
```javascript
// Without ik-seo, urlEndpoint has been defined globally 
<ik-image 
  path="/default-image.jpg"
  :transformation="[{height:300,width:400}]"
/>


// With ik-seo
<ik-image 
  path="/default-image/seo-friendly-file-name.jpg"
  :transformation="[{height:300,width:400}]"
  urlEndpoint="https://ik.imagekit.io/your_imagekit_id/ik-seo"
/>
```
{% endtab %}
{% endtabs %}



