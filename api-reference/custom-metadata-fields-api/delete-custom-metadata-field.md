# Delete custom metadata field

{% swagger method="delete" path="/v1/customMetadataFields/:id" baseUrl="https://api.imagekit.io" summary="Delete a custom metadata field" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="header" name="Authorization" type="string" required="true" %}
base64 encoding of `your_private_api_key:`

**Note the colon in the end.**
{% endswagger-parameter %}

{% swagger-response status="204: OK" description="Empty body is returned." %}

{% endswagger-response %}

{% swagger-response status="404: Not Found" description="If no custom metadata is found with this id, 404 is returned." %}

{% endswagger-response %}
{% endswagger %}

{% hint style="info" %}
Even after deleting a custom metadata field, you cannot create any new custom metadata field with the same `name`.
{% endhint %}

## Examples

{% tabs %}
{% tab title="cURL" %}
```bash
curl -X DELETE "https://api.imagekit.io/v1/customMetadataFields/field_id" \
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

var fieldId = "field_id";
imagekit.deleteCustomMetadataField(
    fieldId,
    function(error, result) {
        if(error) console.log(error);
        else console.log(result);
    }
);
```
{% endtab %}

{% tab title="Python" %}
```python
from imagekitio import ImageKit

imagekit = ImageKit(
    public_key='your_public_api_key',
    private_key='your_private_api_key',
    url_endpoint = 'https://ik.imagekit.io/your_imagekit_id/'
)

delete_custom_metadata_field = imagekit.delete_custom_metadata_field(field_id="field_id")

print("Delete custom metadata field-", delete_custom_metadata_field, end="\n\n")

# Raw Response
print(delete_custom_metadata_field.response_metadata.raw)
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

$customMetadataFieldId = 'field_id';
$deleteCustomMetadataField = $imageKit->deleteCustomMetadataField($customMetadataFieldId);

echo("Delete Custom Metadata Field : " . json_encode($deleteCustomMetadataField));
```
{% endtab %}

{% tab title="Java" %}
```java
ResultNoContent resultNoContent=ImageKit.getInstance().deleteCustomMetaDataField("field_id");
```
{% endtab %}

{% tab title='Ruby' %}
```ruby
imagekitio = ImageKitIo::Client.new("your_private_key", "your_public_key", "your_url_endpoint")
imagekitio.delete_custom_metadata_field(id: 'field_id')
```
{% endtab %}

{% tab title="Go" %}
```go
resp, err := ik.Metadata.DeleteCustomField(ctx, "field_id")

```
{% endtab %}

{% tab title=".Net" %}
```.net
var imagekit = new ImageKit({
    publicKey : "your_public_api_key",
    privateKey : "your_private_api_key",
    urlEndpoint : "https://ik.imagekit.io/your_imagekit_id/"
});
ResultNoContent resultNoContentDel = imagekit.DeleteCustomMetaDataField("field_id");
```
{% endtab %}

{% endtabs %}
