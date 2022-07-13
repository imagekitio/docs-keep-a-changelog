# Get custom metadata field

{% swagger baseUrl="https://api.imagekit.io" path="/v1/customMetadataFields" method="get" summary="Get existing custom metadata fields" %}
{% swagger-description %}
Get a list of all the custom metadata fields.
{% endswagger-description %}

{% swagger-parameter in="header" name="Authorization" required="true" %}
base64 encoding of `your_private_api_key:`

**Note the colon in the end.**
{% endswagger-parameter %}

{% swagger-parameter in="query" name="includeDeleted" %}
Set it to `true` if you want to receive deleted fields as well in the API response.
{% endswagger-parameter %}

{% swagger-response status="200" description="In the response, you will get an array of all the custom metadata fields with each object having field id, name, label and schema." %}
```javascript
[
    {
        "id": "598821f949c0a938d57563dd",
        "name": "brand",
        "label": "brand",
        "schema": {
            "type": "Text",
            "defaultValue": "Nike"
        }
    },
    {
        "id": "865421f949c0a835d57563dd"
        "name": "price",
        "label": "price",
        "schema": {
            "type": "Number",
            "minValue": 1000,
            "maxValue": 3000
        }
    }
]
```
{% endswagger-response %}
{% endswagger %}



## Examples

{% tabs %}
{% tab title="cURL" %}
```basic
curl -X GET "https://api.imagekit.io/v1/customMetadataFields" \
-H 'Content-Type: application/json' \
-u your_private_key:
```
{% endtab %}

{% tab title="Node.js" %}
```javascript
var ImageKit = require("imagekit");

var imagekit = new ImageKit({
    publicKey : "your_public_api_key",
    privateKey : "your_private_api_key",
    urlEndpoint : "https://ik.imagekit.io/your_imagekit_id/"
});

imagekit.getCustomMetadataFields({
    includeDeleted: false
}, function(error, result) {
    if(error) console.log(error);
    else console.log(result);
});
```
{% endtab %}

{% tab title="PHP" %}
```php
use ImageKit\ImageKit;

$public_key = "your_public_api_key";
$your_private_key = "your_private_api_key";
$url_end_point = "https://ik.imagekit.io/your_imagekit_id";

$imageKit = new ImageKit(
    $public_key,
    $your_private_key,
    $url_end_point
);

$includeDeleted = false;
$getCustomMetadataField = $imageKit->getCustomMetadataFields($includeDeleted);

echo("Get Custom Metadata Field : " . json_encode($getCustomMetadataField));
```
{% endtab %}

{% tab title="Java" %}
```java
boolean includeDeleted = false;
ResultCustomMetaDataFieldList resultCustomMetaDataFieldList = ImageKit.getInstance().getCustomMetaDataFields(includeDeleted);
```
{% endtab %}

{% tab title='Ruby' %}
```ruby
imagekitio = ImageKitIo::Client.new("your_private_key", "your_public_key", "your_url_endpoint")
imagekitio.get_custom_metadata_fields(include_deleted: false)
```
{% endtab %}

{% endtabs %}
