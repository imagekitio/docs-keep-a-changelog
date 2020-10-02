# Copy folder

{% api-method method="post" host="https://api.imagekit.io" path="/v1/bulkJobs/copyFolder" %}
{% api-method-summary %}
Copy folder API
{% endapi-method-summary %}

{% api-method-description %}
This will copy one folder into another.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-headers %}
{% api-method-parameter type="string" required=true name="Authorization" %}
base64 encoding of `your_private_api_key:`  
Note the colon in the end
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-body-parameters %}
{% api-method-parameter name="sourceFolderPath" type="string" required=true %}
The full path to the source folder you want to copy. For example - `/path/of/source/folder`.
{% endapi-method-parameter %}

{% api-method-parameter name="destinationPath" type="string" required=true %}
Full path to the destination folder where you want to copy the source folder into. For example - `/path/of/destination/folder/`.
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
On success, you will receive a `jobId` which can be used to get the copy operation's status.
{% endapi-method-response-example-description %}

```javascript
{
    "jobId" : "598821f949c0a938d57563bd"
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=404 %}
{% api-method-response-example-description %}
If no files or folders are found at the specified `sourceFolderPath` then a `404` response is returned.
{% endapi-method-response-example-description %}

```javascript
{
     "message" : "No files & folder found at sourceFolderPath /folder/to/copy",
     "help" : "For support kindly contact us at support@imagekit.io .",
     "reason" : "NO_FILES_FOLDER" 
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

### Response structure and status code

In case of error, you will get an [error code](../api-introduction/#error-codes) along with the error message. On success, you will receive a `200` status code with JSON encoded response containing information about `jobId`. You can use `jobId` to get the status of this job using [bulk job status API](copy-move-folder-status.md). 

### Examples

Here is the example request to understand the API usage.

{% tabs %}
{% tab title="cURL" %}
```bash
curl -X POST "https://api.imagekit.io/v1/bulkJobs/copyFolder" \
-H 'Content-Type: application/json' \
-u your_private_key: -d '
{
	"sourceFolderPath" : "/folder/to/copy",
	"destinationPath" : "/folder/to/copy/into/"
}
'
```
{% endtab %}
{% endtabs %}





