# Update custom metadata field

{% swagger baseUrl="https://api.imagekit.io" path="/v1/customMetadataFields/:id" method="patch" summary="Update an existing custom metadata field" %}
{% swagger-description %}
Update the label or schema of an existing custom metadata field.
{% endswagger-description %}

{% swagger-parameter in="header" name="Authorization" type="string" required="true" %}
base64 encoding of `your_private_api_key:`

**Note the colon in the end.**
{% endswagger-parameter %}

{% swagger-parameter in="body" name="label" type="string" required="false" %}
Label of the metadata field, unique across all non deleted custom metadata fields. This parameter is required if `schema` is not provided.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="schema" type="object" %}
An object that describes the rules for the custom metadata key. This parameter is required if `label` is not provided.


**Note:** `type` cannot be updated and will be ignored if sent with the `schema`. The schema will be validated as per the existing `type`.
{% endswagger-parameter %}

{% swagger-response status="200" description="Custom metadata definition successfully updated. In the response, you will get the field id, field name and, field schema." %}
```javascript
{
    "id": "field_id",
    "name": "price",
    "label": "price",
    "schema": {
        "type": "Number",
        "minValue": 2000,
        "maxValue": 5000
    }
}
```
{% endswagger-response %}

{% swagger-response status="400" description="A 400 response is returned along with an errors object with the key-value pair of all the errors if there is an error in schema object validation." %}
```javascript
{
    "message": "Invalid field object",
    "help": "For support kindly contact us at support@imagekit.io .",
    "errors": {
        "minValue": "should be of type number",
        "maxLength": "not allowed for this type"
    }
}
```
{% endswagger-response %}

{% swagger-response status="404: Not Found" description="If no custom metadata is found with this id, 404 is returned" %}

{% endswagger-response %}
{% endswagger %}

## Examples

{% tabs %}
{% tab title="cURL" %}
```bash
curl -X PATCH "https://api.imagekit.io/v1/customMetadataFields/field_id" \
-H 'Content-Type: application/json' \
-u your_private_key: -d'
{
    "schema": {
        "minValue": 500,
        "maxValue": 2500
    }
}
'
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
imagekit.updateCustomMetadataField(
    fieldId,
    {
        schema: {
            minValue: 500,
            maxValue: 2500
        }
    }, 
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

update_custom_metadata_fields = imagekit.update_custom_metadata_fields(field_id="id", 
                                                 options=UpdateCustomMetadataFieldsRequestOptions(label="test-update",
                                                  schema=CustomMetadataFieldsSchema(
                                                      min_value=100,
                                                      max_value=200))
)

print("Update custom metadata field-", update_custom_metadata_fields, end="\n\n")

# Raw Response
print(update_custom_metadata_fields.response_metadata.raw)

# print the label of updated custom metadata fields
print(update_custom_metadata_fields.label)

# print the schema's min value of updated custom metadata fields
print(update_custom_metadata_fields.schema.min_value)
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

$body = [
    "schema" => [
        "minValue" => 500
        "maxValue" => 2500
    ]
];

$customMetadataFieldId = 'field_id';
$updateCustomMetadataField = $imageKit->updateCustomMetadataField($customMetadataFieldId, $body);

echo("Update Custom Metadata Field : " . json_encode($updateCustomMetadataField));
```
{% endtab %}
{% tab title="Java" %}
```java

CustomMetaDataFieldSchemaObject schemaObject = new CustomMetaDataFieldSchemaObject();
schemaObject.setMinValue(500);
schemaObject.setMaxValue(2500);

CustomMetaDataFieldUpdateRequest customMetaDataFieldUpdateRequest = new CustomMetaDataFieldUpdateRequest();
customMetaDataFieldUpdateRequest.setId("field_id");
customMetaDataFieldUpdateRequest.setLabel("");
customMetaDataFieldUpdateRequest.setSchema(schemaObject);

ResultCustomMetaDataField resultCustomMetaDataField = ImageKit.getInstance().updateCustomMetaDataFields(customMetaDataFieldUpdateRequest);

```
{% endtab %}

{% tab title='Ruby'%}
```ruby
imagekitio = ImageKitIo::Client.new("your_private_key", "your_public_key", "your_url_endpoint")
imagekitio.update_custom_metadata_field(
  id: 'field_id', #required
  schema: {   #required if label not available
    minValue: 500,
    maxValue: 2500
  }
)
```
{% endtab %}

{% tab title="Go" %}
```go
// Atleast Label or Schema is required
resp, err := ik.Metadata.UpdateCustomField(ctx, "field_id", UpdateCustomFieldParam{
    Label: "Cost",
    Schema: {
        MinValue: 100,
        MaxValue: 2500,
    },
})
```
{% endtab %}

{% tab title=".Net" %}
```.net
var imagekit = new ImageKit({
    publicKey : "your_public_api_key",
    privateKey : "your_private_api_key",
    urlEndpoint : "https://ik.imagekit.io/your_imagekit_id/"
});
CustomMetaDataFieldUpdateRequest requestUpdateModel = new CustomMetaDataFieldUpdateRequest
{
    Id = "field_Id",
};
CustomMetaDataFieldSchemaObject updateschema = new CustomMetaDataFieldSchemaObject
{
    minValue = 500,
    maxValue = 2500
};
requestUpdateModel.schema = updateschema;
ResultCustomMetaDataField resultCustomMetaDataFieldUpdate = imagekit.UpdateCustomMetaDataFields(requestUpdateModel);
```
{% endtab %}

{% endtabs %}
