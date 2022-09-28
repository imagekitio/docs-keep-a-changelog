# Signed URLs

A signed URL is a secure URL that can be generated only by you using your [account's private key](../../api-reference/api-introduction/api-keys.md#private-key). There are certain use cases where you will need to use signed URLs:

* You have turned on the "[Restrict unsigned URLs](./#restricting-unsigned-urls)" setting from the dashboard.
* You watermark all your images using ImageKit.io parameters to protect original assets. You do not want anyone to access the original image by removing the [watermark specific transformation ](../image-transformations/overlay.md#image-overlay)from the image URL.​
* You want certain image URLs in your application to be accessible only for a specific time period in the future.
* You are trying to access a [private image](private-images.md).

When generating signed URLs, use the private key available within the [Developer section](https://imagekit.io/dashboard/developer/api-keys) on the dashboard. Signing the URLs adds additional query parameters to ensure that image transformations cannot be altered from the URL. If someone tries to modify the image transformation or the image URL or use it beyond its intended expiry time, a `401 Unauthorised` response status code is returned.

A signed URL would be similar to :

https://ik.imagekit.io/your_imagekit_id/path-to-image.jpg?ik-s=`generatedURLsignature`\&ik-t=`UTCtimestamp`

{% hint style="info" %}
If you want to create a signed URL that uses a Web Proxy origin, you must encode the complete URL of the input image before signing it. For example, instead of using `https://example.com/image.jpg`as input for the signed URL, you should use `https%3A%2F%2Fexample.com%2Fimage.jpg`.
{% endhint %}

## Generating Signed URLs

You can create a signed URL using [server-side SDKs](../../api-reference/api-introduction/sdk.md#server-side-sdks).

{% tabs %}
{% tab title="Node.js" %}
```javascript
var imageURL = imagekit.url({
    path : "/default-image.jpg",
    queryParameters : {
        "v" : "123"
    },
    transformation : [{
        "height" : "300",
        "width" : "400"
    }],
    signed : true,
    expireSeconds : 300
});
```
{% endtab %}

{% tab title="Python" %}
```python
image_url = imagekit.url({
    "path": "/default-image",
    "query_parameters": {
                "v": "123"
    },
    "transformation": [{
        "height": "300",
        "width": "400"
    }],
    "signed": True,
    "expire_seconds": 300
});
```
{% endtab %}

{% tab title="PHP" %}
```php
$imageURL = $imageKit->url(array(
    "path" => "/default-image.jpg",
    "queryParameters" => array(
        "v" => "123",
    ),
    "transformation" => array(
        array(
            "height" => "300",
            "width" => "400"
        ),
    ),
    "signed" => true,
    "expireSeconds" => 300,
));
```
{% endtab %}

{% tab title="Java" %}
```java
List<Map<String, String>> transformation=new ArrayList<Map<String, String>>();
Map<String, String> scale=new HashMap<>();
scale.put("height","600");
scale.put("width","400");

transformation.add(format);

Map<String, Object> options=new HashMap();
options.put("path","/default-image.jpg");
options.put("signed",true);
options.put("expireSeconds",300);
String url = ImageKit.getInstance().getUrl(options);
```
{% endtab %}

{% tab title="Ruby" %}
```ruby
imagekitio = ImageKitIo::Client.new("your_private_key", "your_public_key", "your_url_endpoint")
image_url = imagekitio.url({
    path: "/default-image",
    query_parameters: {
                "v": "123"
    },
    transformation: [{
        height: "300",
        width: "400"
    }],
    signed: True,
    expire_seconds: 300
})
```
{% endtab %}
{% endtabs %}

### Pseudo-code for signed URL generation

If you want to generate the signed URL yourself, refer to the pseudo-code below.

{% hint style="info" %}
The value of signature i.e. `ik-s` should be in lowercase. 
{% endhint %}

```javascript
// Assume we have an image URL
var imageUrl = "https://ik.imagekit.io/your_imagekit_id/tr:w-400:rotate-91/sample/testing-file.jpg";

// This is our endpoint
var urlEndpoint = "https://ik.imagekit.io/your_imagekit_id";

// Make sure urlEndpoint has a trailing slash (/)
if(urlEndpoint[urlEndpoint.length - 1] != "/") {
    urlEndpoint = urlEndpoint + "/"
}

// Let's say we want to expire image in 300 seconds, so expireTimestamp (UTC timestamp) would be
var expiryTimestamp = parseInt(new Date().getTime() / 1000, 10) + 300;

// Remove the urlEndpoint from image URL
var str = imageUrl.replace(urlEndpoint,"");
// str will be tr:w-400:rotate-91/sample/testing-file.jpg

// Append the expiryTimestamp in above str
str = str + expiryTimestamp
// str will be tr:w-400:rotate-91/sample/testing-file.jpg9999999999

// Calcualte the signature using your priviate key 
var signature = crypto.createHmac('sha1', "your_private_key").update(str).digest('hex');

// Add ik-t and ik-s query parameters in the url
var finalImageUrl = imageUrl + "?ik-t=" + expiryTimestamp + "&ik-s=" + signature;
```
