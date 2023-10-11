# Shopify

![](<../.gitbook/assets/image (2).png>)

Shopify is a e-commerce platform that has gained quite a lot of traction in the last few years given the great feature set, ease of use and nominal pricing that comes with starting an e-commerce store on Shopify.

## Integration steps

Integrating ImageKit on Shopify store allows you to improve your image quality and delivery speed.

{% hint style="info" %}
The integration with Shopify involves making some changes to your website’s theme files.
{% endhint %}

Here are the steps to optimize images on your Shopify website

### Step 1: Configure origin in ImageKit.io dashboard

Configure a [web server](../integration/configure-origin/web-server-origin.md) origin based on where your images are stored. In Shopify, this is your CDN base URL i.e. `https://cdn.shopify.com`, `https://cdn2.shopify.com` or `https://cdn.shopifycloud.com`. You can check this under the network panel tab in Chrome like shown in the screenshot below:

![](../.gitbook/assets/adyhsz7y9lb8hxpbunr2.jpg)

### Step 2: Fetch image through ImageKit.io endpoint

Let's quickly fetch the image using ImageKit.io [URL-endpoint](../integration/url-endpoints.md) and see if it's working.

When you add the first origin in your account, it automatically becomes accessible through the [default URL-endpoint](../integration/url-endpoints.md#default-url-endpoint), that is `https://ik.imagekit.io/your_imagekit_id`. Otherwise, you will have to [configure an existing](../integration/url-endpoints.md#image-origin-preference) URL endpoint or create a new one to fetch images from this newly added origin.

If your old image URL was [`https://cdn.shopify.com/img/s/files/1/1510/6482/t/22/assets/logo-mobile.png`](https://cdn.shopify.com/img/s/files/1/1510/6482/t/22/assets/logo-mobile.png) , then the same image should be accessible through ImageKit.io URL-endpoint, i.e., [`https://ik.imagekit.io/your_imagekit_id/img/s/files/1/1510/6482/t/22/assets/logo-mobile.png`](https://ik.imagekit.io/your\_imagekit\_id/img/s/files/1/1510/6482/t/22/assets/logo-mobile.png)

{% hint style="danger" %}
**Unable to fetch image?**\
\*\*\*\*Contact [support@imagekit.io](mailto:support@imagekit.io) if you are not able to fetch the image as explained above. In such a situation, do not move to step 3 as this could break your website images.
{% endhint %}

### Step 3: Create settings\_schema.json

If the above image did work correctly, we can now proceed to make the changes in our Shopify settings and theme files to switch the image delivery and optimization to ImageKit.

From your Shopify admin, click **Online Store** :arrow\_right:**Themes**. Find the theme you want to edit, click the `...` button, and then click **Edit HTML/CSS**.

Under **Config**, click `settings_schema.json` and copy the code below as the last section of config file and hit save.

```javascript
{
    "name": "ImageKit",
    "settings": [
        {
            "type": "paragraph",
            "content": "Check out ImageKit's [ImageKit.io and Shopify integration](https://docs.imagekit.io/imagekit-docs/shopify) to learn more about this."
        },
        {
            "type": "checkbox",
            "id": "enableImageKit",
            "label": "Enable Imagekit"
        },
        {
            "type": "text",
            "id": "imagekitUrl",
            "label": "ImageKit url endpoint",
            "info":"The url endpoint you set within ImageKit. Example: `https://ik.imagekit.io/your_imagekit_id/`."
        },
        {
            "type": "text",
            "id": "imagekitShopifyCdnUrl",
            "label": "Shopify CDN domain",
            "default":"//cdn.shopify.com",
            "info":"Do not change this unless you have a proxy in place. Not sure? Leave it as is."
        }
    ]
}
```

### Step 4: Create imagekit.liquid file

Create a new file `imagekit.liquid` under Snippets directory. Copy the code below into that file, and save it.

```javascript

<div data-gb-custom-block data-tag="capture">

  

<div data-gb-custom-block data-tag="if">

    

<div data-gb-custom-block data-tag="for" data-0='1' data-1='1' data-2='1' data-3='1' data-4='0.1'>

      

<div data-gb-custom-block data-tag="unless">

        {{ src }}
        

<div data-gb-custom-block data-tag="break"></div>

      

</div>

      

<div data-gb-custom-block data-tag="assign" data-0=',' data-1=',' data-2=',' data-3=',' data-4=',' data-5=',' data-6=',' data-7=',' data-8=',' data-9=',' data-10=',' data-11=',' data-12=','></div>

    

<div data-gb-custom-block data-tag="if" data-0='0' data-1='0' data-2='0' data-3='0' data-4='0' data-5='0' data-6='0' data-7='0' data-8='0'>

    {{ src }}
        

<div data-gb-custom-block data-tag="break"></div>

    

</div>

      

<div data-gb-custom-block data-tag="assign" data-0='0' data-1='0' data-2='0' data-3='0' data-4='0' data-5='0' data-6='0' data-7='0' data-8='0' data-9='0' data-10='0' data-11='0' data-12='0' data-13='0' data-14='0' data-15='0' data-16='0' data-17='0'></div>

      

<div data-gb-custom-block data-tag="for">

        

<div data-gb-custom-block data-tag="if">

          

<div data-gb-custom-block data-tag="assign"></div>

          

<div data-gb-custom-block data-tag="break"></div>

        

</div>

      

</div>

    

<div data-gb-custom-block data-tag="assign"></div>

    

<div data-gb-custom-block data-tag="unless">

        {{ src }}
        

<div data-gb-custom-block data-tag="break"></div>

      

</div>

    

<div data-gb-custom-block data-tag="assign"></div>

      

<div data-gb-custom-block data-tag="assign"></div>

      

<div data-gb-custom-block data-tag="assign" data-0='-1' data-1='-1' data-2='-1' data-3='-1' data-4='-1' data-5='-1' data-6='-1' data-7='-1' data-8='-1' data-9='-1' data-10='-1' data-11='-1' data-12='-1' data-13='-1' data-14='-1' data-15='-1' data-16='1'></div>

      

<div data-gb-custom-block data-tag="assign"></div>

      

<div data-gb-custom-block data-tag="assign" data-0='1' data-1='1' data-2='1' data-3='1' data-4='1' data-5='1' data-6='1' data-7='1' data-8='1' data-9='1' data-10='1' data-11='1' data-12='1' data-13='1' data-14='1' data-15='1' data-16='1' data-17='1' data-18='1' data-19='1' data-20='1' data-21='1' data-22='1' data-23='1' data-24='1' data-25='1' data-26='1' data-27='1' data-28='1' data-29='1' data-30='1' data-31='1' data-32='1' data-33='1' data-34='1' data-35='1' data-36='1' data-37='1' data-38='1' data-39='1' data-40='1' data-41='1' data-42='1' data-43='1'></div>

      

<div data-gb-custom-block data-tag="if" data-0='/' data-1='/' data-2='/' data-3='/'>

        

<div data-gb-custom-block data-tag="assign" data-0='0' data-1='0' data-2='0' data-3='0' data-4='0' data-5='0' data-6='0' data-7='0' data-8='0' data-9='0' data-10='0' data-11='0' data-12='0' data-13='0' data-14='0' data-15='0' data-16='0' data-17='0' data-18='0' data-19='0' data-20='0' data-21='0' data-22='0' data-23='0' data-24='0' data-25='0' data-26='0' data-27='0' data-28='0' data-29='0' data-30='0' data-31='0' data-32='0' data-33='0' data-34='0' data-35='0' data-36='0' data-37='0' data-38='0'></div>

      

</div>

    

<div data-gb-custom-block data-tag="assign"></div>

    {{ newSrc | default:src }}
    

</div>

  

<div data-gb-custom-block data-tag="else"></div>

    {{ src }}
  

</div>

</div>

{{ IMAGEKIT | strip | replace:'  ' | strip_newlines }}
```

### Step 5: Enable ImageKit.io

Navigate to **Online store** :arrow\_right:**Themes** :arrow\_right:**Customize theme**. In the sidebar, under general settings open ImageKit and enable it. Fill out the below two fields:

* Default URL endpoint - It should be `https://ik.imagekit.io/your_imagekit_id`
* Shopify CDN domain - Its value should be `//cdn2.shopify.com,//cdn.shopify.com`
* Hit the "Save" button.

### Step 6: Edit theme files

Now we get to editing your theme files. We first need to find out the files which are responsible for the output of image on your store and start editing them.

{% hint style="warning" %}
**Backup your theme files**\
\*\*\*\*Before making changes in these files, it is recommended that you download and save them securely to be able to restore it later, in case of error.
{% endhint %}

Here are a couple of examples indicating the change that needs to be made in the theme files. You can follow similar steps to change all of your theme files.

**Example 1**

```markup
Before
<img class="grid-view-item__image" src="

<div data-gb-custom-block data-tag="product"></div>

" alt="{{ product.featured_image.alt }}">

After

<div data-gb-custom-block data-tag="assign"></div>

<img class="grid-view-item__image" src="

<div data-gb-custom-block data-tag="include" data-0='imagekit'></div>" alt="{{ product.featured_image.alt }}">
```

**Example 2**

```markup
Before:
<img src="<div data-gb-custom-block data-tag="featured_img_src"></div>" alt="{{ featured_img_alt }}" id="FeaturedImage-{{ section.id }}" class="product-featured-img<div data-gb-custom-block data-tag="if"> js-zoom-enabled</div>

">

After:

<div data-gb-custom-block data-tag="assign"></div>

<img src="

<div data-gb-custom-block data-tag="include" data-0='imagekit'></div>" alt="{{ featured_img_alt }}" id="FeaturedImage-{{ section.id }}" class="product-featured-img<div data-gb-custom-block data-tag="if"> js-zoom-enabled</div>">
```

**Example 3**

If there is a tag inside image URL and you cannot simply assign it, use [capture](https://help.shopify.com/en/themes/liquid/tags/variable-tags#capture)

```markup
Before:
<img width="{{ image.width }}" height="{{ image.height }}" src="{{ image | img_url: '50x50' }}" class="lazyload attachment-shop_single size-shop_single sp-post-image" alt="{{image.alt}}" title="{{product.title}}" 
data-src="{{ img_url }}" 
data-large_image="<div data-gb-custom-block data-tag="-include" data-0='gl_image_format' data-1='true' data-2='true'></div>

" data-large_image_width="{{ image.width }}" data-large_image_height="{{ image.height }}" 
data-widths="[180, 360, 540, 720, 900, 1080, 1296, 1512, 1728, 2048]" data-aspectratio="{{ image.aspect_ratio }}" data-sizes="auto">

After:

<div data-gb-custom-block data-tag="assign" data-0='50x50' data-1='50x50' data-2='50x50' data-3='50x50' data-4='50' data-5='50' data-6='0'></div>

<div data-gb-custom-block data-tag="assign"></div>

<div data-gb-custom-block data-tag="capture">

<div data-gb-custom-block data-tag="-include" data-0='gl_image_format' data-1='true' data-2='true'></div>

</div>

<img width="{{ image.width }}" height="{{ image.height }}" src="

<div data-gb-custom-block data-tag="include" data-0='imagekit'></div>" class="lazyload attachment-shop_single size-shop_single sp-post-image" alt="{{image.alt}}" title="{{product.title}}" 
data-src="<div data-gb-custom-block data-tag="include" data-0='imagekit'></div>" 
data-large_image="<div data-gb-custom-block data-tag="include" data-0='imagekit'></div>" data-large_image_width="{{ image.width }}" data-large_image_height="{{ image.height }}" 
data-widths="[180, 360, 540, 720, 900, 1080, 1296, 1512, 1728, 2048]" data-aspectratio="{{ image.aspect_ratio }}" data-sizes="auto">
```

## FAQ

### Can I disable ImageKit.io with a single click?

Yes you can enable and disable ImageKit.io on your Shopify store with a single click. Navigate to **Online store** > **Themes** > **Customize theme**. In the sidebar, under general settings open ImageKit.io and enable/disable it.

### Can ImageKit.io automatically detect the right image dimension and load it?

No ImageKit.io only changes the base URL of your images because there is no way the server would have knowledge of your website layout. However, just by loading images through ImageKit.io URL-endpoint, your images are automatically optimized for format and quality.

### How do I make sure my integration is working?

Once you are done editing these files, save these files. Now refresh the webpage for your Shopify store and check the image URLs. They should now load from URLs beginning with `https://ik.imagekit.io/your_imagekit_id`. You can use the Chrome Developer Tools to check that all the images are being loaded via ImageKit.io and that all images are loading correctly. If you find that images on a particular page or section is still being served from Shopify, then find out the responsible theme file and edit it as well.
