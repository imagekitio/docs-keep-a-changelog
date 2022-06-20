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

## Examples

{% tabs %}
{% tab title="cURL" %}
```bash
curl -X DELETE "https://api.imagekit.io/v1/customMetadataFields/6152fc9a2fd12044cb4cefe2" \
-H 'Content-Type: application/json' \
-u your_private_key:
```
{% endtab %}
{% tab title='Ruby' %}
```ruby
imagekitio = ImageKitIo::Client.new("your_private_key", "your_public_key", "your_url_endpoint")
imagekitio.delete_custom_metadata_field(id: '6152fc9a2fd12044cb4cefe2')
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

$deleteCustomMetadataField = $imageKit->deleteCustomMetadataField($customMetadataFieldId);

echo("Delete Custom Metadata Field : " . json_encode($deleteCustomMetadataField));
```
{% endtab %}
{% endtabs %}
