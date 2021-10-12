# List and search files

{% swagger baseUrl="https://api.imagekit.io" path="/v1/files" method="get" summary="List and search file API" %}
{% swagger-description %}
This API can list all the uploaded files and folders in your ImageKit.io media library. You can fine-tune your query by specifying various filters by generating a query string in a Lucene-like syntax and provide this generated string as the value of the 

`searchQuery`

.
{% endswagger-description %}

{% swagger-parameter in="header" name="Authorization" type="string" %}
base64 encoding of 

`your_private_api_key:`

\




**Note the colon in the end.**
{% endswagger-parameter %}

{% swagger-parameter in="query" name="type" type="string" %}
Limit search to either 

`file`

 or 

`folder`

.  Pass 

`all`

 to include both files and folders in search results.

\




\




**Default value**

 \- 

`file`
{% endswagger-parameter %}

{% swagger-parameter in="query" name="sort" type="string" %}
You can sort based on the following fields: 

\


1\. name - 

`ASC_NAME`

 or 

`DESC_NAME`

\


2\. createdAt - 

`ASC_CREATED`

 or 

`DESC_CREATED`

\


3\. updatedAt - 

`ASC_UPDATED`

 or 

`DESC_UPDATED`

\


4\. height - 

`ASC_HEIGHT`

 or 

`DESC_HEIGHT`

\


5\. width - 

`ASC_WIDTH`

 or 

`DESC_WIDTH`

\


6\. size - 

`ASC_SIZE`

 or 

`DESC_SIZE`

\




\




**Default value**

 \- 

`ASC_CREATED`
{% endswagger-parameter %}

{% swagger-parameter in="query" name="path" type="string" %}
Folder path if you want to limit the search within a specific folder. For example, 

`/sales-banner/`

 will only search in folder sales-banner.
{% endswagger-parameter %}

{% swagger-parameter in="query" name="searchQuery" type="string" %}
Query string in a Lucene-like query language. Learn more about the query expression later in this section.

\




**Note**

: When the 

_searchQuery_

 parameter is present, the following query parameters will have no effect on the result:

\


1\. tags

\


2\. type

\


3\. name
{% endswagger-parameter %}

{% swagger-parameter in="query" name="fileType" type="string" %}
Type of files to include in the result set. Accepts three values:

\




`all`

 \- include all types of files in the result set

\




`image`

 \- only search in image type files

\




`non-image`

 \- only search in files that are not images, e.g., JS or CSS or video files.

\




\




**Default value **

\- 

`all`
{% endswagger-parameter %}

{% swagger-parameter in="query" name="limit" type="string" %}
The maximum number of results to return in response:

\




\




**Minimum value**

 \- 

`1`

\




**Maximum value**

 \- 

`1000`

\




**Default value**

 \- 

`1000`
{% endswagger-parameter %}

{% swagger-parameter in="query" name="skip" type="string" %}
The number of results to skip before returning results.

\




**Minimum value**

 \- 

`0`

 

\




**Default value**

 \- 

`0`
{% endswagger-parameter %}

{% swagger-response status="200" description="" %}
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
      "mime": "image/jpeg",
      "width": 100,
      "height": 100,
      "size": 100,
      "hasAlpha": false,
      "createdAt": "2019-08-24T06:14:41.313Z",
      "updatedAt": "2019-08-24T06:14:41.313Z"
  },
	...more items
]
```
{% endswagger-response %}
{% endswagger %}

## Advanced search queries

The `searchQuery` parameter can be used to apply advanced filters to your search. 

### Basic examples

{% tabs %}
{% tab title="Basic usage" %}
You can query based on a single field. For example:

```
createdAt > "7d"
```

This will return all files which are created in the last 7 days.
{% endtab %}

{% tab title="Using AND/OR operator" %}
You can combine multiple conditions using the `AND` and `OR` operators. For example:

```markup
createdAt > "7d" AND name: "file-name"
```

This will return all files created within the last 7 days with name starting with `file-name`.
{% endtab %}

{% tab title="Grouping multiple queries" %}
You can use parenthesis `(` and `)` to group multiple queries and create complex search filters. For example:

```
(size < "1mb" AND width > 500) OR (tags IN ["summer-sale","banner"])
```
{% endtab %}
{% endtabs %}

### Search based on file and folder names

For example, let's say you have uploaded two files `red-dress-summer.jpg` and `red-dress-winter.jpg` in the [media library](../../media-library/overview/).

{% hint style="info" %}
The name match is case-sensitive.
{% endhint %}

{% tabs %}
{% tab title="Exact match" %}
To find a file or folder using the exact name, use `=` operator. For example:

```
name = "red-dress-summer.jpg"
```

This will only return the file with the name `red-dress-summer.jpg`
{% endtab %}

{% tab title="Begins with match" %}
To find a file or folder using a prefix, you can use `:` operator. For example:

```
name : "red-dress"
```

This will return both `red-dress-summer.jpg` and `red-dress-winter.jpg`.
{% endtab %}
{% endtabs %}

### Search based on upload date

You can filter using `createdAt` and `updatedAt` to search based on the first uploaded or last modified time.

`createdAt` and `updatedAt` accepts ISO 8601 format string or relative unit.

{% tabs %}
{% tab title="ISO 8601 format" %}
The API supports a string in ISO 8601 format.

* `YYYY-MM-DD` - When no time is provided, it is set to 00:00:00 UTC by default.
* `YYYY-MM-DDTHH:MM:SS`

For example:

```
createdAt < 2020-01-01
createdAt < "2020-01-01T12:12:12"
```
{% endtab %}

{% tab title="Relative units" %}
The API supports relative time units, e.g:

* `1h` - one hour in the past
* `2d` - two days in the past
* Similarly `3w`, `4m` etc.

Example usage:

```
createdAt < 2d (createdAt should be before two days ago)
createdAt < 2w (createdAt should be before two weeks ago)
createdAt < 2m (createdAt should be before two months ago)
createdAt < 2y (createdAt should be before two years ago)  
```
{% endtab %}
{% endtabs %}

### Supported parameters

| **Field**    | **Supported operators**                                                                                                 | Examples                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| ------------ | ----------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| name         | <ul><li>=</li><li>:</li><li>IN</li><li>NOT =</li><li>NOT IN</li></ul>                                                   | <p>Accepts a string value in quotes. For example:</p><p></p><p><code>name = "red-dress.jpg" </code>will return all <br>files &#x26; folders with the exact name <code>red-dress.jpg</code>.</p><p></p><p><code>name: "red-dress"</code> will return all files &#x26; folders </p><p>with a name starting with <code>red-dress</code>.</p><p></p><p><code>name IN ["red-dress.jpg", "red-dress.png"]</code> will return</p><p>all files &#x26; folders with the name either <code>red-dress.jpg</code> or</p><p><code>red-dress.png</code>.</p><p></p><p><code>name NOT = "red-dress.jpg"</code> will return all files and folders</p><p>with a name other than <code>red-dress.jpg</code>.</p> |
| tags         | <ul><li>IN</li><li>NOT IN</li></ul>                                                                                     | <p>Accepts an array of string values.</p><p></p><p><code>tags IN ["summer-collection", "sale"]</code> will return all files that have either <code>summer-collection</code> or <code>sale</code> tag set.</p><p></p><p><code>tags NOT IN ["big-banner"]</code> will return all files that do not have <code>big-banner</code> tag set.</p>                                                                                                                                                                                                                                                                                                                                                     |
| type         | <ul><li>=</li><li>IN</li><li>NOT =</li><li>NOT IN</li></ul>                                                             | <p>Possible values are <code>file</code> or <code>folder</code> in quotes or in the array.</p><p></p><p><code>type = "file"</code> will only return files in the search result.<br></p><p><code>type = "folder"</code> will only return folders in the search result.<br></p><p><code>type IN ["file", "folder"]</code> will return both files and folders in the search result.</p>                                                                                                                                                                                                                                                                                                           |
| createdAt    | <ul><li>=</li><li>&#x3C;</li><li>&#x3C;=</li><li>></li><li>>=</li><li>IN</li><li>NOT =</li><li>NOT IN</li></ul>         | <p>Accepts a string value in ISO 8601 format or relative time units e.g <code>1h</code>, <code>2d</code>, <code>3w</code>, or <code>4m</code>. <br></p><p><code>createdAt > "2020-01-01"</code> will return all files first uploaded <br>after 1 Jan 2020 at 00:00 hours in UTC.</p><p></p><p><code>createdAt > "2020-01-01T12:12:12"</code> will return all files first uploaded after 1 Jan 2020 12:12:12 hours in UTC.</p><p></p><p><code>createdAt > "7d"</code> will return all files first uploaded in the last 7 days.</p>                                                                                                                                                              |
| updatedAt    | <ul><li>=</li><li>&#x3C;</li><li>&#x3C;=</li><li>></li><li>>=</li><li>IN</li><li>NOT =</li><li>NOT IN</li></ul>         | <p>Accepts a string value in ISO 8601 format or relative time units e.g <code>1h</code>, <code>2d</code>, <code>3w</code>, or <code>4m</code>. <br></p><p><code>updatedAt > "2020-01-01"</code> will return all files last modified <br>after 1 Jan 2020 at 00:00 hours in UTC.</p><p></p><p><code>updatedAt > "2020-01-01T12:12:12"</code> will return all files last modified after 1 Jan 2020 12:12:12 hours in UTC.</p><p></p><p><code>updatedAt > "7d"</code> will return all files last modified in the last 7 days.</p>                                                                                                                                                                 |
| height       | <ul><li>=</li><li>&#x3C;</li><li>&#x3C;=</li><li>></li><li>>=</li><li>IN</li><li>NOT =</li><li>NOT IN</li></ul>         | <p>Accepts a numeric value e.g. <code>500</code>, <code>200</code> etc. This is only applicable for image-type assets.</p><p></p><p><code>height > 200</code> will return all image files with a height greater than 200px.</p><p></p><p><code>height &#x3C;= 400</code> will return all image files with a height less than or equal to 400px.</p>                                                                                                                                                                                                                                                                                                                                            |
| width        | <ul><li>=</li><li>&#x3C;</li><li>&#x3C;=</li><li>> </li><li>>=</li><li>IN</li><li>NOT =</li><li>NOT IN</li></ul>        | <p>Accepts a numeric value e.g. <code>500</code>, <code>200</code> etc. This is only applicable for image-type assets.</p><p></p><p><code>width > 200</code> will return all image files with a width greater than 200px.</p><p></p><p><code>width &#x3C;= 400</code> will return all image files with a width less than or equal to 400px.size</p>                                                                                                                                                                                                                                                                                                                                            |
| size         | <p></p><ul><li>=</li><li>&#x3C;</li><li>&#x3C;=</li><li>> </li><li>>=</li><li>IN</li><li>NOT =</li><li>NOT IN</li></ul> | <p>Accepts a numeric value e.g. <code>500</code>, <code>200</code> or string e.g. <code>1mb</code>, <code>10kb</code> etc.</p><p></p><p><code>size > 1024</code> will return all assets with a file size greater than 1024 bytes.<br></p><p><code>size &#x3C;= "1mb"</code> will return all assets with a file size less than or equal to 1MB.</p>                                                                                                                                                                                                                                                                                                                                             |
| format       | <ul><li>=</li><li>IN</li></ul>                                                                                          | <p>Accepts a string value.<br><br>Allowed values are  <code>jpg</code>, <code>webp</code>, <code>png</code>, <code>gif</code>, <code>svg</code>, <code>avif</code>, <code>pdf</code>, <code>js</code>, <code>woff2</code>, <code>woff</code>, <code>ttf</code>, <code>otf</code>, <code>eot</code>, <code>css</code>, <code>txt</code>, <code>mp4</code>, <code>webm</code>, <code>mov</code>, <code>swf</code>, <code>ts</code>, <code>m3u8</code>, <code>ico</code>.</p><p><br><code>format = "jpg"</code> will return all JPG image files.</p>                                                                                                                                              |
| private      | <ul><li>=</li></ul>                                                                                                     | <p>Accepts a boolean value i.e. <code>true</code> or <code>false</code> without quotes.</p><p></p><p><code>private = true</code> will return all files marked as private during upload.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| transparency | <ul><li>=</li></ul>                                                                                                     | <p>Accepts a boolean value i.e. <code>true</code> or <code>false</code> without quotes. This is only applicable to images.</p><p></p><p><code>transparency = true</code> will return all image files that have an alpha layer. However, the presence of the alpha layer does not guarantee transparency if all pixels in the alpha layer have the value 1.</p>                                                                                                                                                                                                                                                                                                                                 |

## Understanding response

In case of an error, you will get an [error code](../api-introduction/#error-codes) along with the error message. On success, you will receive a `200` status code with the list of files in JSON-encoded response body.

The JSON-encoded response has an array of items. Each item can have the following properties.

| Property name     | Description                                                                                                                                                                       |
| ----------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| fileId            | The unique fileId of the uploaded file.                                                                                                                                           |
| type              | Type of item. It can be either `file` or `folder`.                                                                                                                                |
| name              | Name of the file or folder.                                                                                                                                                       |
| filePath          | The relative path of the file. In the case of an image, you can use this path to construct different transformations.                                                             |
| tags              | The array of tags associated with the image. If no tags are set, it will be `null`.                                                                                               |
| isPrivateFile     | Is the file marked as private. It can be either `true` or `false`.                                                                                                                |
| customCoordinates | <p>Value of custom coordinates associated with the image in the format <code>x,y,width,height</code>. If customCoordinates are not defined, then it is <code>null</code>.<br></p> |
| url               | A publicly accessible URL of the file.                                                                                                                                            |
| thumbnail         | In the case of an image, a small thumbnail URL.                                                                                                                                   |
| fileType          | The type of file could be either `image` or `non-image`.                                                                                                                          |
| mime              | MIME Type of the file. For example - `image/jpeg`                                                                                                                                 |
| height            | Height of the image in pixels (Only for images)                                                                                                                                   |
| width             | Width of the image in pixels (Only for Images)                                                                                                                                    |
| size              | Size of the image file in Bytes                                                                                                                                                   |
| hasAlpha          | TODO                                                                                                                                                                              |
| createdAt         | The date and time when the file was first uploaded. The format is `YYYY-MM-DDTHH:mm:ss.sssZ`                                                                                      |
| updatedAt         | The date and time when the file was last updated. The format is `YYYY-MM-DDTHH:mm:ss.sssZ`                                                                                        |

## Examples

Here are some example requests to understand the API usage.

### Fetch 10 files uploaded in the media library

Fetch the first 10 files uploaded in the media library. To change the order, you can use `sort` option.

{% tabs %}
{% tab title="cURL" %}
```bash
curl -X GET 'https://api.imagekit.io/v1/files?skip=0&limit=10' \
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

### Advance search using searchQuery

List all files uploaded in the last 7 days with a file size greater than 2MB. We will use the following value for `searchQuery` parameter.

`createdAt >= "7d" AND size > "2mb"`

{% tabs %}
{% tab title="cURL" %}
```bash
curl -X GET 'https://api.imagekit.io/v1/files' \
-G --data-urlencode "searchQuery=createdAt >= \"7d\" AND size > \"2mb\"" \
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
    searchQuery : 'createdAt >= "7d" AND size > "2mb"'
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

list_files = imagekit.list_files({'searchQuery': 'createdAt >= "7d" AND size > "2mb"'})

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
    "searchQuery" => 'createdAt >= "7d" AND size > "2mb"',
));

echo ("List files : " . json_encode($listFiles));
```
{% endtab %}

{% tab title="Java" %}
```java
Map<String , String> options=new HashMap<>();
options.put("searchQquery",'createdAt >= "7d" AND size > "2mb"');
ResultList resultList=ImageKit.getInstance().getFileList(options);
```
{% endtab %}

{% tab title="Ruby" %}
```ruby
imagekitio = ImageKit::ImageKitClient.new("your_private_key", "your_public_key", "your_url_endpoint")
list_files = imagekitio.list_files({searchQuery : 'createdAt >= "7d" AND size > "2mb"'})
```
{% endtab %}
{% endtabs %}

### Search media library files by name

List all files with filename `file-name.jpg`. We will use the following value of the`searchQuery` parameter. The match is always case-sensitive.

`name="file-name.jpg"`

{% tabs %}
{% tab title="cURL" %}
```bash
curl -X GET 'https://api.imagekit.io/v1/files' \
-G --data-urlencode "searchQuery=name=\"file-name.jpg\"" \
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
    searchQuery : 'name="file-name.jpg"'
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

list_files = imagekit.list_files({'searchQuery': 'name="file-name.jpg"'})

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
    "searchQuery" => 'name="file-name.jpg"',
));

echo ("List files : " . json_encode($listFiles));
```
{% endtab %}

{% tab title="Java" %}
```java
Map<String , String> options=new HashMap<>();
options.put("searchQquery",'name="file-name.jpg"');
ResultList resultList=ImageKit.getInstance().getFileList(options);
```
{% endtab %}

{% tab title="Ruby" %}
```ruby
imagekitio = ImageKit::ImageKitClient.new("your_private_key", "your_public_key", "your_url_endpoint")
list_files = imagekitio.list_files({searchQuery : 'name="file-name.jpg"'})
```
{% endtab %}
{% endtabs %}

### List files within a specific folder

List all files at a specific location. 

{% hint style="info" %}
When using path parameter, the search is limited to only one level. The files within nested folders are not returned.
{% endhint %}

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

### Simple search files by tags without using searchQuery

This will list all the files which have either `sale` or `summer` tag.

{% tabs %}
{% tab title="cURL" %}
```bash
curl -X GET 'https://api.imagekit.io/v1/files?tags=sale,summer' \
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

### Search all PNG files

This will list all `PNG` type files. We will use the following value of `searchQuery`

`format="png"`

{% tabs %}
{% tab title="cURL" %}
```bash
curl -X GET 'https://api.imagekit.io/v1/files' \
-G --data-urlencode "searchQuery=format=\"png\"" \
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
    searchQuery : 'format="png"'
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

list_files = imagekit.list_files({"searchQuery": 'format="png"'})

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
    "searchQuery" => 'format="png"',
));

echo ("List files : " . json_encode($listFiles));
```
{% endtab %}

{% tab title="Java" %}
```java
Map<String , String> options=new HashMap<>();
options.put("searchQuery",'format="png"');
ResultList resultList=ImageKit.getInstance().getFileList(options);
```
{% endtab %}

{% tab title="Ruby" %}
```ruby
imagekitio = ImageKit::ImageKitClient.new("your_private_key", "your_public_key", "your_url_endpoint")
list_files = imagekitio.list_files({searchQuery : 'format="png"'})
```
{% endtab %}
{% endtabs %}
