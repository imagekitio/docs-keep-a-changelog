# List and search files

{% api-method method="get" host="https://api.imagekit.io" path="/v1/files" %}
{% api-method-summary %}
List and search file API
{% endapi-method-summary %}

{% api-method-description %}
This API can list all the uploaded files in your ImageKit.io media library. For searching and filtering, you can use query parameters as described below.
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
{% api-method-parameter name="path" type="string" required=false %}
Folder path if you want to limit the search within a specific folder. For example, `/sales-banner/` will only search in folder sales-banner.
{% endapi-method-parameter %}

{% api-method-parameter name="fileType" type="string" required=false %}
Type of files to include in the result set. Accepts three values:  
`all` - include all types of files in result set  
`image` - only search in image type files  
`non-image` - only search in files that are not image, e.g., JS or CSS or video files.  
  
**Default value** - `all`
{% endapi-method-parameter %}

{% api-method-parameter name="tags" type="string" required=false %}
A comma-separated list of tags. Files matching any of the tags are included in the result response. If no tag is matched, the file is not included in the result set.  
  
Example - `t-shirt,round-neck,sale2019` will return any files which have either of these three tags, `t-shirt` or `round-neck` or `sale2019`.
{% endapi-method-parameter %}

{% api-method-parameter name="includeFolder" type="string" required=false %}
Whether to include folders in search results or not. By default, only files are searched.  
Accepts `true` and `false`. If this is set to `true` then `tags` and `fileType` parameters are ignored.
{% endapi-method-parameter %}

{% api-method-parameter name="name" type="string" required=false %}
The name of the file or folder.
{% endapi-method-parameter %}

{% api-method-parameter name="limit" type="string" required=false %}
The maximum number of results to return in response:  
  
**Minimum value** - `1`  
**Maximum value** - `1000`  
**Default value** - `1000`
{% endapi-method-parameter %}

{% api-method-parameter name="skip" type="string" required=false %}
The number of results to skip before returning results.  
**Minimum value** - `0`   
**Default value** - `0`
{% endapi-method-parameter %}
{% endapi-method-query-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
[
	{
	    "fileId" : "598821f949c0a938d57563bd",
      "type": "file",
      "name": "file1.jpg",
      "filePath": "/images/products/file1.jpg",
      "tags": ["t-shirt","round-neck","sale2019"],
      "isPrivateFile" : false,
      "customCoordinates" : null,
      "url": "https://ik.imagekit.io/your_imagekit_id/images/products/file1.jpg",
      "thumbnail": "https://ik.imagekit.io/your_imagekit_id/tr:n-media_library_thumbnail/images/products/file1.jpg",
      "fileType": "image",
      "createdAt": "2019-08-24T06:14:41.313Z"
  },
	...more items
]
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

### Response structure and status code \(application/JSON\)

In case of error, you will get an [error code](../api-introduction/#error-codes) along with the error message. On success, you will receive a `200` status code with the list of files in JSON-encoded response body.

### Understanding response

The JSON-encoded response has an array of items. Each item can have the following properties.

| Property name | Description |
| :--- | :--- |
| fileId | The unique fileId of the uploaded file.  |
| type | Type of item. It can be either `file` or `folder`. |
| name | Name of the file or folder. |
| filePath | The relative path of the file. In the case of an image, you can use this  path to construct different transformations. |
| tags | Array of tags associated with the image. If no tags are set, it will be `null`. |
| isPrivateFile | Is the file marked as private. It can be either `true` or `false`. |
| customCoordinates | Value of custom coordinates associated with the image in format `x,y,width,height`. If customCoordinates are not defined then it is `null`.  |
| url | A publicly accessible URL of the file. |
| thumbnail | In case of an image, a small thumbnail URL. |
| fileType | The type of file, it could be either `image` or `non-image`. |
| createdAt | The date and time when the file was first uploaded. The format is `YYYY-MM-DDTHH:mm:ss.sssZ` |

## Examples

Here are some example requests to understand the API usage.

### Fetch 10 files uploaded in media library

{% tabs %}
{% tab title="cURL" %}
```bash
curl -X GET "https://api.imagekit.io/v1/files?skip=0&limit=10" \
-u your_private_api_key:
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

imagekit.listFiles({
    skip : 0,
    limit : 10
}, function(error, result) { 
    if(error) console.log(error);
    else console.log(result);
});
```
{% endtab %}

{% tab title="Python" %}
```python
from imagekitio import ImageKit

imagekit = ImageKit(
    private_key='your_public_api_key',
    public_key='your_private_api_key',
    url_endpoint = 'https://ik.imagekit.io/your_imagekit_id/'
)

list_files = imagekit.list_files({"skip": 0, "limit": 10})

print("List files-", "\n", list_files)
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

$listFiles = $imageKit->listFiles(array(
    "skip" => 0,
    "limit" => 10,
));

echo ("List files : " . json_encode($listFiles));
```
{% endtab %}

{% tab title="Java" %}
```java
Map<String , String> options=new HashMap<>();
options.put("skip","0");
options.put("limit", "10");
ResultList resultList=ImageKit.getInstance().getFileList(options);
```
{% endtab %}

{% tab title="Ruby" %}
```ruby
imagekitio = ImageKit::ImageKitClient.new("your_private_key", "your_public_key", "your_url_endpoint")
list_files = imagekitio.list_files({skip: 0, limit: 5})
```
{% endtab %}
{% endtabs %}

### Search media library files by name

{% tabs %}
{% tab title="cURL" %}
```bash
curl -X GET "https://api.imagekit.io/v1/files?name=main-banner.jpg" \
-u your_private_api_key:
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

imagekit.listFiles({
    name : "main-banner.jpg"
}, function(error, result) { 
    if(error) console.log(error);
    else console.log(result);
});
```
{% endtab %}

{% tab title="Python" %}
```python
from imagekitio import ImageKit

imagekit = ImageKit(
    private_key='your_public_api_key',
    public_key='your_private_api_key',
    url_endpoint = 'https://ik.imagekit.io/your_imagekit_id/'
)

list_files = imagekit.list_files({"name": "main-banner.jpg"})

print("List files-", "\n", list_files)
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

$listFiles = $imageKit->listFiles(array(
    "name" => "main-banner.jpg",
));

echo ("List files : " . json_encode($listFiles));
```
{% endtab %}

{% tab title="Java" %}
```java
Map<String , String> options=new HashMap<>();
options.put("name","main-banner.jpg");
ResultList resultList=ImageKit.getInstance().getFileList(options);
```
{% endtab %}

{% tab title="Ruby" %}
```ruby
imagekitio = ImageKit::ImageKitClient.new("your_private_key", "your_public_key", "your_url_endpoint")
list_files = imagekitio.list_files({name : "main-banner.jpg"})
```
{% endtab %}
{% endtabs %}

### List files within a specific folder

{% tabs %}
{% tab title="cURL" %}
```bash
curl -X GET "https://api.imagekit.io/v1/files?path=products" \
-u your_private_api_key:
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

imagekit.listFiles({
    path : "products"
}, function(error, result) { 
    if(error) console.log(error);
    else console.log(result);
});
```
{% endtab %}

{% tab title="Python" %}
```python
from imagekitio import ImageKit

imagekit = ImageKit(
    private_key='your_public_api_key',
    public_key='your_private_api_key',
    url_endpoint = 'https://ik.imagekit.io/your_imagekit_id/'
)

list_files = imagekit.list_files({"path": "products"})

print("List files-", "\n", list_files)
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

$listFiles = $imageKit->listFiles(array(
    "path" => "products",
));

echo ("List files : " . json_encode($listFiles));
```
{% endtab %}

{% tab title="Java" %}
```java
Map<String , String> options=new HashMap<>();
options.put("path","products");
ResultList resultList=ImageKit.getInstance().getFileList(options);
```
{% endtab %}

{% tab title="Ruby" %}
```ruby
imagekitio = ImageKit::ImageKitClient.new("your_private_key", "your_public_key", "your_url_endpoint")
list_files = imagekitio.list_files({path : "products"})
```
{% endtab %}
{% endtabs %}

### Search files by tags

{% tabs %}
{% tab title="cURL" %}
```bash
curl -X GET "https://api.imagekit.io/v1/files?tags=sale,summer" \
-u your_private_api_key:
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

imagekit.listFiles({
    tags : ["sale","summer"]
}, function(error, result) { 
    if(error) console.log(error);
    else console.log(result);
});
```
{% endtab %}

{% tab title="Python" %}
```python
from imagekitio import ImageKit

imagekit = ImageKit(
    private_key='your_public_api_key',
    public_key='your_private_api_key',
    url_endpoint = 'https://ik.imagekit.io/your_imagekit_id/'
)

list_files = imagekit.list_files({"tags": ["sale","summer"]})

print("List files-", "\n", list_files)
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

$listFiles = $imageKit->listFiles(array(
    "tags" => implode(",", array("sale", "summer")),
));

echo ("List files : " . json_encode($listFiles));
```
{% endtab %}

{% tab title="Java" %}
```java
Map<String , String> options=new HashMap<>();
options.put("tags","sale,summer");
ResultList resultList=ImageKit.getInstance().getFileList(options);
```
{% endtab %}

{% tab title="Ruby" %}
```ruby
imagekitio = ImageKit::ImageKitClient.new("your_private_key", "your_public_key", "your_url_endpoint")
list_files = imagekitio.list_files({tags : "sale,summer"})
```
{% endtab %}
{% endtabs %}

### Limit the search to only image type files

{% tabs %}
{% tab title="cURL" %}
```bash
curl -X GET "https://api.imagekit.io/v1/files?fileType=image" \
-u your_private_api_key:
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

imagekit.listFiles({
    fileType : "image"
}, function(error, result) { 
    if(error) console.log(error);
    else console.log(result);
});
```
{% endtab %}

{% tab title="Python" %}
```python
from imagekitio import ImageKit

imagekit = ImageKit(
    private_key='your_public_api_key',
    public_key='your_private_api_key',
    url_endpoint = 'https://ik.imagekit.io/your_imagekit_id/'
)

list_files = imagekit.list_files({"fileType": "image"})

print("List files-", "\n", list_files)
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

$listFiles = $imageKit->listFiles(array(
    "fileType" => "image",
));

echo ("List files : " . json_encode($listFiles));
```
{% endtab %}

{% tab title="Java" %}
```java
Map<String , String> options=new HashMap<>();
options.put("fileType","image");
ResultList resultList=ImageKit.getInstance().getFileList(options);
```
{% endtab %}

{% tab title="Ruby" %}
```ruby
imagekitio = ImageKit::ImageKitClient.new("your_private_key", "your_public_key", "your_url_endpoint")
list_files = imagekitio.list_files({fileType : "image"})
```
{% endtab %}
{% endtabs %}

