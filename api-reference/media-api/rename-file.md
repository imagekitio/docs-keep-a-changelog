# Rename file

{% api-method method="put" host="https://api.imagekit.io" path="/v1/files/rename" %}
{% api-method-summary %}
Rename file API
{% endapi-method-summary %}

{% api-method-description %}
You can programmatically rename an already existing file in the media library using rename file API.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-headers %}
{% api-method-parameter name="Authorization" type="string" required=true %}
base64 encoding of `your_private_api_key:`   
**Note the colon in the end.**
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-body-parameters %}
{% api-method-parameter name="filePath" type="string" required=true %}
The full path of the file you want to rename. For example - /path/to/file.jpg
{% endapi-method-parameter %}

{% api-method-parameter name="newFileName" type="string" required=true %}
The new name of the file. A filename can contain:  
  
- Alphanumeric Characters: `a-z` , `A-Z` , `0-9` \(including Unicode letters, marks, and numerals in other languages\).   
- Special Characters: `.` `_` and `-`  
  
Any other character, including space, will be replaced by `_`.  
{% endapi-method-parameter %}

{% api-method-parameter name="purgeCache" type="boolean" required=false %}
Option to purge cache for the old file URL.   
  
When set to `true`, it will internally issue a purge cache request on CDN to remove cached content on the old URL. E.g. if an old file was accessible at  -`https://ik.imagekit.io/demo/old-filename.jpg`, a purge cache request will be issued to remove the CDN cache for this URL. This purge request is counted against your monthly purge quota.  
  
**Default value** - `false`
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
On success, you will receive `purgeRequestId` in the response body, which can be used to get the purge request status. This is only sent if the `purgeCache` is set to `true` in the request. Otherwise, the response is an empty JSON.
{% endapi-method-response-example-description %}

```javascript
// When purgeCache is set to true
{
    purgeRequestId: "598821f949c0a938d57563bd"
}

// When purgeCache is not set or set to false, response is empty
{}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=207 %}
{% api-method-response-example-description %}
In case `purgeCache` is set to `true` in the request and your total purge request count has exceeded the quota, we will rename the file but won't be able to purge the old URL from the CDN.
{% endapi-method-response-example-description %}

```javascript
{
    "message" : "File renamed successfully but we could not purge the CDN cache for old URL because of rate limits on purge API.",
    "help" : "For support kindly contact us at support@imagekit.io .",
    "reason" : "PURGE_FAILED" 
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=404 %}
{% api-method-response-example-description %}
If no file is found at the specified `filePath` in the media library, then a 404 response is returned.
{% endapi-method-response-example-description %}

```javascript
{
     "message" : "No file found in media library at filePath /path/to/file.jpg",
     "help" : "For support kindly contact us at support@imagekit.io .",
     "reason" : "FILE_MISSING" 
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=409 %}
{% api-method-response-example-description %}
If a file with `newFileName` already exist in the same location, a `409` response is returned.
{% endapi-method-response-example-description %}

```javascript
{
     "message" : "File with name newFileName already exists at the same location.",
     "help" : "For support kindly contact us at support@imagekit.io .",
     "reason" : "FILE_ALREADY_EXISTS" 
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

### Response structure and status code

In case of an error, you will get an [error code](../api-introduction/#error-codes) along with the error message. On success, you will receive a `200` status code with the JSON-encoded response body.

The response depends upon the value of `purgeCache` in the request. If `purgeCache` is set to `true`, the response contains `purgeRequestId`, which can be used to get the [purge cache status](purge-cache-status.md).

{% tabs %}
{% tab title="purgeCache=true" %}
```javascript
// When purgeCache is set to true, we send the purge request ID.
// You can use this to get purge status
{
    purgeRequestId: "598821f949c0a938d57563bd"
}
```
{% endtab %}

{% tab title="purgeCache=false or unset" %}
```javascript
// When purgeCache is set to false or not set
{}
```
{% endtab %}
{% endtabs %}

### Example

Here is an example request to understand API usage.

{% tabs %}
{% tab title="cURL" %}
```bash
curl -X PUT "https://api.imagekit.io/v1/files/rename" \
-H 'Content-Type: application/json' \
-u your_private_key: -d '
{
	"filePath" : "/path/to/old-file-name.jpg",
	"newFileName" : "new-file-name.jpg",
	"purgeCache": false
}
'
```
{% endtab %}
{% endtabs %}

