# Create custom metadata field

{% swagger baseUrl="https://api.imagekit.io" path="/v1/customMetadataFields" method="post" summary="Add custom metadata field" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="header" name="Authorization" type="string" required="true" %}
base64 encoding of `your_private_api_key:`

**Note the colon in the end.**
{% endswagger-parameter %}

{% swagger-parameter in="body" name="name" type="string" required="true" %}
Name of the metadata field, unique across all (deleted or not deleted) custom metadata fields
{% endswagger-parameter %}

{% swagger-parameter in="body" name="label" type="string" required="true" %}
Label of the metadata field, unique across all non deleted custom metadata fields
{% endswagger-parameter %}

{% swagger-parameter in="body" name="schema" type="object" required="true" %}
An object that describes the rules for the custom metadata key.
{% endswagger-parameter %}

{% swagger-response status="201" description="Custom metadata field successfully created. In the response, you will get the field id, field name and, field schema." %}
```javascript
{
    "id": "598821f949c0a938d57563dd",
    "name": "price",
    "label": "price",
    "schema": {
        "type": "Number",
        "minValue": 1000,
        "maxValue": 3000
    }
}
```
{% endswagger-response %}

{% swagger-response status="400" description="A 400 response is returned along with an errors object if there is an error in validating field object." %}
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
{% endswagger %}

### Allowed values in the schema object

| Parameter name  | Type                                                                 | Required                                          | Descriptions                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| --------------- | -------------------------------------------------------------------- | ------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| type            | `enum`                                                               | Yes                                               | <p>Type of the field<br><br>Allowed values - <code>Text</code>, <code>Textarea</code>, <code>Number</code>, <code>Date</code>, <code>Boolean</code>, <code>SingleSelect</code>,  <code>MultiSelect</code></p><p></p><p><code>Date</code> value should be an ISO8601 string</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| selectOptions   | An `array` consisting values of type `string`, `number` or `boolean` | Only if `type` is `SingleSelect` or `MultiSelect` | <p>An array of options to select from. <br><br>Example - <code>["small", "medium", "large", 30, 40, true]</code></p><p></p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| defaultValue    | `string`                                                             | Only if `type` is `isValueRequired`               | <p>The default value for the field<br></p><p><code>type</code><em> </em>constraints : <br></p><p>Should be of the same type as that provided in the<code>type</code> enum.<br></p><p>For <code>SingleSelect</code>, should be one of the values provided in <code>selectOptions</code></p><p></p><p>For <code>MultiSelect</code>, should be an <code>array</code> containing only values provided in <code>selectOptions</code></p><p></p><p>For <code>Date</code> or <code>Number</code>, should be >= <code>minValue</code> (if provided) and &#x3C;= <code>maxValue</code> (if provided)</p><p></p><p>For <code>Text</code> or <code>Textarea</code>, should be >= <code>minLength</code> (if provided) and &#x3C;= <code>maxLength</code> (if provided)``</p> |
| isValueRequired | `boolean`                                                            | No                                                | Sets field as required                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| minValue        | `string`                                                             | No                                                | <p>Minimum value of the field</p><p></p><p>Allowed only if <code>type</code> is <code>Date</code> or <code>Number</code></p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| maxValue        | `string`                                                             | No                                                | <p>Maximum value of the field</p><p></p><p>Allowed only if <code>type</code> is <code>Date</code> or <code>Number</code></p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| minLength       | `number`                                                             | No                                                | <p>Minimum length of string</p><p></p><p>Allowed only if <code>type</code> is <code>Text</code> or <code>Textarea</code></p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| maxLength       | `number`                                                             | No                                                | <p>Maximum length of string</p><p></p><p>Allowed only if <code>type</code> is <code>Text</code> or <code>Textarea</code></p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |

## Examples&#x20;

Here is the example request to understand the API usage.

{% tabs %}
{% tab title="cURL" %}
```bash
# The unique id of the created custom metadata schema is returned with this api along with key name and schema object.
curl -X POST "https://api.imagekit.io/v1/customMetadataFields" \
-H 'Content-Type: application/json' \
-u your_private_key: -d'
{
    "name": "price",
    "label": "price",
    "schema": {
        "type": "Number",
        "minValue": 1000,
        "maxValue": 3000
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

imagekit.createCustomMetadataField(
    {
        name: "price",
        label: "price",
        schema: {
            type: "Number",
            minValue: 1000,
            maxValue: 3000
        }
    }, 
    function(error, result) {
        if(error) console.log(error);
        else console.log(result);
    }
);
```
{% endtab %}

{% tab title="Ruby" %}
```ruby
imagekitio = ImageKitIo::Client.new("your_private_key", "your_public_key", "your_url_endpoint")
imagekitio.create_custom_metadata_field(
  name: 'price',
  label: 'price',
  schema: {
    type: 'Number',
    minValue: 1000,
    maxValue: 3000
  }
)
```
{% endtab %}
{% endtabs %}
