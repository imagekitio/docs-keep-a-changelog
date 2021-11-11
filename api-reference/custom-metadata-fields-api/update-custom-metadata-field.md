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
    "id": "598821f949c0a938d57563dd",
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
curl -X PATCH "https://api.imagekit.io/v1/customMetadataFields/6152fc9a2fd12044cb4cefe2" \
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
{% endtabs %}
