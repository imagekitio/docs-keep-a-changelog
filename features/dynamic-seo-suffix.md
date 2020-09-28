---
description: >-
  Give your images an SEO-friendly name without modifying the actual file name
  in the storage
---

# SEO-friendly image URL

### ImageKit helps you create SEO-friendly URLs dynamically

Let's first understand the basics. Image SEO starts with the file name. Google uses the URL path as well as the file name to understand your images. Google recommends organizing your image content so that URLs are constructed logically.

For example, consider the following image of the Eiffel Tower:

![](../.gitbook/assets/eiffel-tower.jpg)

The image URL should have "Eiffel Tower" instead of "DSC1234.jpg". 

{% hint style="danger" %}
**Bad image URL**  
https://ik.imagekit.io/places-to-visit-in-paris/DSC1234.jpg
{% endhint %}

{% hint style="success" %}
**SEO-friendly image URL**  
https://ik.imagekit.io/places-to-visit-in-paris/eiffel-tower.jpg
{% endhint %}

### Dynamic SEO-friendly suffix \(ik-seo\)

In cases when you cannot modify actual file names of already stored images, ImageKit helps you create SEO-friendly URLs dynamically.

For example, let say you have the following image of the Eiffel Tower.

```text
http://ik.imagekit.io/demo/places-to-visit-in-paris/DSC1234.jpg
```

Here, `http://ik.imagekit.io/demo/` is your [URL-endpoint](../integration/url-endpoints.md). In case you are using a [custom domain name](using-custom-domain.md), it could have been `https://images.your-domain.com`.

You can dynamically use **eiffel-tower.jpg** as the file name using `ik-seo` parameter. For example:

```text
http://ik.imagekit.io/demo/ik-seo/places-to-visit-in-paris/DSC1234/eiffel-tower.jpg
```

So the following URL:  
  
`http://ik.imagekit.io/demo/ik-seo/places-to-visit-in-paris/DSC1234/eiffel-tower.jpg` 

will fetch the same image as below URL:  
  
`http://ik.imagekit.io/demo/places-to-visit-in-paris/DSC1234.jpg`

Essentially

`your-url-endpoint/path-of-image/old-file-name.extension` 

becomes:

`your_url_endpoint/ik-seo/path-of-image/old-file-name/seo-friendly-file-name.extension`

