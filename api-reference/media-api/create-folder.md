# Create folder

{% api-method method="post" host="https://api.imagekit.io" path="/v1/folder/" %}
{% api-method-summary %}
Create folder API
{% endapi-method-summary %}

{% api-method-description %}
This will create a new folder. You can specify the folder name and location of the parent folder where this new folder should be created. 
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-headers %}
{% api-method-parameter name="Authorization" type="string" required=true %}
base64 encoding of `your_private_api_key:`  
**Note the colon in the end.**
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-query-parameters %}
{% api-method-parameter name="folderName" type="string" required=true %}
The folder will be created with this name. All characters except alphabets and numbers will be replaced by an underscore i.e. `_`
{% endapi-method-parameter %}

{% api-method-parameter name="parentFolderPath" type="string" required=true %}
The folder where the new folder should be created, for root use `/` else `containing/folder/`  
**Note:** If any folder\(s\) is not present in the `parentFolderPath` parameter, it will be automatically created. For example, if you pass `/product/images/summer`, then `product`, `images`, and `summer` folders will be created if they don't already exist.
{% endapi-method-parameter %}
{% endapi-method-query-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=201 %}
{% api-method-response-example-description %}
Folder created successfully
{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

### Response structure and status code

On success, you will receive a `201` status code with an empty body.

## Examples

Here is the example request to understand the API usage.

{% tabs %}
{% tab title="cURL" %}
```bash
curl -X POST "https://api.imagekit.io/v1/folder/" \
-H 'Content-Type: application/json' \
-u your_private_key: -d '
{
	"folderName" : "new_folder",
	"parentFolderPath" : "source/folder/path"
}
'
```
{% endtab %}
{% endtabs %}

