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
https://ik.imagekit.io/DSC1234.jpg
{% endhint %}

{% hint style="success" %}
**SEO-friendly image URL**  
https://ik.imagekit.io/eiffel-tower.jpg
{% endhint %}

### Dynamic SEO suffix \(ik-seo\)

In cases when you cannot modify the file names of already stored images, ImageKit helps you create SEO-friendly URLs dynamically.

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

your\_url\_endpoint/`ik-seo`/old-file-name/seo-friendly-file-name.extension

### Accessing file that is stored in a nested folder

If your file is stored inside a nested folder e.g.

`https://ik.imagekit.io/demo/path/of/folder/old-file-name.jpg` . 

You can still dynamically add an SEO-friendly suffix like this:

`https://ik.imagekit/io/demo/ik-seo/path/of/folder/old-file-name/seo-friendly-file-name.jpg`

