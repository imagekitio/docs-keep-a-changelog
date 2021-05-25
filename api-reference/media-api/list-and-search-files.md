# List and search files

{% api-method method="get" host="https://api.imagekit.io" path="/v1/files" %}
{% api-method-summary %}
List and search file API
{% endapi-method-summary %}

{% api-method-description %}
This API can list all the uploaded files and folders in your ImageKit.io media library. You can fine-tune your query by specifying various filters by generating a query string in a Lucene-like syntax and provide this generated string as the value of the `searchQuery`.
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
{% api-method-parameter name="sort" type="string" required=false %}
You can sort based on the following fields:   
1. name - `ASC_NAME` or `DESC_NAME`  
2. createdAt - `ASC_CREATED` or `DESC_CREATED`  
3. updatedAt - `ASC_UPDATED` or `DESC_UPDATED`  
4. height - `ASC_HEIGHT` or `DESC_HEIGHT`  
5. width - `ASC_WIDTH` or `DESC_WIDTH`  
6. size - `ASC_SIZE` or `DESC_SIZE`
{% endapi-method-parameter %}

{% api-method-parameter name="path" type="string" required=false %}
Folder path if you want to limit the search within a specific folder. For example, `/sales-banner/` will only search in folder sales-banner.
{% endapi-method-parameter %}

{% api-method-parameter name="searchQuery" type="string" required=false %}
Query string in a Lucene-like query language. Learn more about the query expression later in this section.  
**Note**: When the _searchQuery_ parameter is present, the following query parameters will have no effect on the result:  
1. tags  
2. includeFolder  
3. name
{% endapi-method-parameter %}

{% api-method-parameter name="fileType" type="string" required=false %}
Type of files to include in the result set. Accepts three values:  
`all` - include all types of files in the result set  
`image` - only search in image type files  
`non-image` - only search in files that are not images, e.g., JS or CSS or video files.  
  
**Default value** - `all`
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
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

### Advanced search queries

The `searchQuery` parameter can be used to apply advanced filters to your search. For example, the following search query would limit the search results to items created on or after a week ago and whose size is greater than 50 kb.

```text
createdAt > 1w AND size > 50kb
```

Here is the [complete list of parameters](list-and-search-files.md#complete-list-of-parameters) supported in `searchQuery`.

A few more examples:

#### 1. Single Field

```markup
name = red_dress.jpg
```

Use the '**='** \(equals\) ****operator when you want to find exact matches. The above query will return files whose name is exactly `red_dress.jpg`.

```markup
name : red
```

Use the ':' \(colon\) operator when you want to find matches with a specified prefix. The above query will return files whose name starts with the string 'red'. For example, both `red-dress.jpg` and `red-shirt.png` will be returned by the above query.

{% hint style="info" %}
The Filename match is case-sensitive.
{% endhint %}

Similarly, the following table specifies all the possible operators on a single field:

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>Operator</b>
      </th>
      <th style="text-align:left"><b>What it does</b>
      </th>
      <th style="text-align:left"><b>Example</b>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">
        <p>=</p>
        <p>(equals)</p>
      </td>
      <td style="text-align:left">Matches items with fields that are exactly equal to the specified value</td>
      <td
      style="text-align:left">name = red_dress.jpg</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p>:</p>
        <p>(colon)</p>
      </td>
      <td style="text-align:left">Matches items with fields whose values begin with the specified value</td>
      <td
      style="text-align:left">name : red</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p>&lt;</p>
        <p>(less than)</p>
      </td>
      <td style="text-align:left">Matches items with fields whose values are less than the specified value</td>
      <td
      style="text-align:left">size &lt; 10kb</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p>&lt;=</p>
        <p>(less than equal)</p>
      </td>
      <td style="text-align:left">Matches items with fields whose values are less than or equal to the specified
        value</td>
      <td style="text-align:left">size &lt;= 10kb</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p>&gt;</p>
        <p>(greater than)</p>
      </td>
      <td style="text-align:left">Matches items with fields whose values are greater than the specified
        value</td>
      <td style="text-align:left">size &gt; 1mb</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p>&gt;=</p>
        <p>(greater than equal)</p>
      </td>
      <td style="text-align:left">Matches items with fields whose values are greater than or equal to the
        specified value</td>
      <td style="text-align:left">size &gt;= 1mb</td>
    </tr>
    <tr>
      <td style="text-align:left">IN (in operator)</td>
      <td style="text-align:left">Matches items with fields whose value is exactly equal to one of the specified
        values</td>
      <td style="text-align:left">color IN [&quot;red&quot;, &quot;blue&quot;]</td>
    </tr>
  </tbody>
</table>

#### 2. Combining multiple Fields using AND and OR operators

It is possible to combine multiple filters in a single query by combining them using boolean logical operators AND and OR.

```markup
size < 1mb AND width > 1000
```

The above query will return results for which the size is less than 1MB and the width is greater than 1000 px.

```markup
size < 1mb OR height < 5000
```

The above query will return results for which the size is less than 1MB, or the height is less than 5000 px.

It is also possible to create more complex queries using parenthesis. For example:

```markup
(size < 1mb AND width > 500) OR (size < 3mb AND height > 500)
```

Here, the expressions inside the parenthesis will be evaluated first and then combined using a logical OR operator. 

#### 3. NOT operator

You can specify the NOT operator preceding any other operator to negate that particular expression. For example:

```markup
color NOT IN ["red", "blue"]
```

The above query will return results for which the color field's value is not "red" or "blue".

#### 4. Dates

You can specify dates \(and times\) in one of the following three formats:

* **YYYY-MM-DD** \(no time - when no time is provided, it is set to 00:00:00 UTC by default\)
* **YYYY-MM-DD HH:MM:SS** \(date and time separated by a space\)
* **YYYY-MM-DDTHH:MM:SS** \(date and time separated by a T\)

Alternatively, you can use quantifiers for days, weeks, months, and years along with a number. `2d` translates to the date that was two days ago. `2w` translates to the date that was two weeks ago. And similarly for `2m`, and `2y`.  
  
For example, the following dates are all valid:

```markup
createdAt < 2020-01-01
createdAt < 2020-01-01T12:12:12
createdAt < 2020-01-01 12:12:12
createdAt < 2d (createdAt should be before two days ago)
createdAt < 2w (createdAt should be before two weeks ago)
createdAt < 2m (createdAt should be before two months ago)
createdAt < 2y (createdAt should be before two years ago)
```

#### 5. Arrays

Arrays of values can be specified as well, just with one restriction.  
All non-numeric values inside an array **MUST be written within double-quotes.**

This is a valid array value:

✅`tags IN ["small", "inventory"]`     
  
This is **NOT** a valid array value:

❌ `tags IN ['small', 'inventory']`   

### Complete List of Parameters

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>Field</b>
      </th>
      <th style="text-align:left"><b>Operators Supported</b>
      </th>
      <th style="text-align:left">Possible Values</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">name</td>
      <td style="text-align:left">= (equals), : (colon), IN (in operator)</td>
      <td style="text-align:left">Any string value(s).</td>
    </tr>
    <tr>
      <td style="text-align:left">type</td>
      <td style="text-align:left">= (equals), : (colon), IN (in operator)</td>
      <td style="text-align:left"><code>&quot;file&quot;</code> or <code>&quot;folder&quot;</code> or <code>[&quot;file&quot;, &quot;folder&quot;]</code>.</td>
    </tr>
    <tr>
      <td style="text-align:left">createdAt</td>
      <td style="text-align:left">= (equals), &lt; (less than), &lt;= (less than equal), &gt; (greater than),
        &gt;= (greater than equal), IN (in operator)</td>
      <td style="text-align:left">Any date value(s) in any of the formats specified above in the dates section.</td>
    </tr>
    <tr>
      <td style="text-align:left">updatedAt</td>
      <td style="text-align:left">= (equals), &lt; (less than), &lt;= (less than equal), &gt; (greater than),
        &gt;= (greater than equal), IN (in operator)</td>
      <td style="text-align:left">Any date value(s) in any of the formats specified above in the dates section.</td>
    </tr>
    <tr>
      <td style="text-align:left">height</td>
      <td style="text-align:left">= (equals), &lt; (less than), &lt;= (less than equal), &gt; (greater than),
        &gt;= (greater than equal), IN (in operator)</td>
      <td style="text-align:left">Any numeric value(s).</td>
    </tr>
    <tr>
      <td style="text-align:left">width</td>
      <td style="text-align:left">= (equals), &lt; (less than), &lt;= (less than equal), &gt; (greater than),
        &gt;= (greater than equal), IN (in operator)</td>
      <td style="text-align:left">Any numeric value(s).</td>
    </tr>
    <tr>
      <td style="text-align:left">size</td>
      <td style="text-align:left">= (equals), &lt; (less than), &lt;= (less than equal), &gt; (greater than),
        &gt;= (greater than equal), IN (in operator)</td>
      <td style="text-align:left">Any numeric value(s).</td>
    </tr>
    <tr>
      <td style="text-align:left">format</td>
      <td style="text-align:left">= (equals), : (colon), IN (in operator)</td>
      <td style="text-align:left">
        <p>Supported formats: <code>jpg</code>, <code>webp</code>, <code>png</code>, <code>gif</code>, <code>svg</code>, <code>avif</code>, <code>pdf</code>, <code>js</code>, <code>woff2</code>, <code>woff</code>, <code>ttf</code>, <code>otf</code>, <code>eot</code>, <code>css</code>, <code>txt</code>, <code>mp4</code>, <code>webm</code>, <code>mov</code>, <code>swf</code>, <code>ts</code>, <code>m3u8</code>, <code>ico</code>
        </p>
        <p>Example values: <code>&quot;jpg&quot;</code>, <code>[&quot;jpg&quot;, &quot;pdf&quot;]</code>.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">private</td>
      <td style="text-align:left">= (equals), : (colon), IN (in operator)</td>
      <td style="text-align:left">A boolean value i.e. <code>true</code> or <code>false</code>.</td>
    </tr>
    <tr>
      <td style="text-align:left">transparency</td>
      <td style="text-align:left">= (equals), : (colon), IN (in operator)</td>
      <td style="text-align:left">A boolean value i.e. <code>true</code> or <code>false</code>.</td>
    </tr>
    <tr>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
    </tr>
  </tbody>
</table>

### Response structure and status code \(application/JSON\)

In case of an error, you will get an [error code](../api-introduction/#error-codes) along with the error message. On success, you will receive a `200` status code with the list of files in JSON-encoded response body.

### Understanding response

The JSON-encoded response has an array of items. Each item can have the following properties.

| Property name | Description |
| :--- | :--- |
| fileId | The unique fileId of the uploaded file. |
| type | Type of item. It can be either `file` or `folder`. |
| name | Name of the file or folder. |
| filePath | The relative path of the file. In the case of an image, you can use this path to construct different transformations. |
| tags | The array of tags associated with the image. If no tags are set, it will be `null`. |
| isPrivateFile | Is the file marked as private. It can be either `true` or `false`. |
| customCoordinates | Value of custom coordinates associated with the image in the format `x,y,width,height`. If customCoordinates are not defined, then it is `null`.  |
| url | A publicly accessible URL of the file. |
| thumbnail | In the case of an image, a small thumbnail URL. |
| fileType | The type of file could be either `image` or `non-image`. |
| mime | MIME Type of the file. For example - `image/jpeg` |
| height | Height of the image in pixels \(Only for images\) |
| width | Width of the image in pixels \(Only for Images\) |
| size | Size of the image file in Bytes |
| hasAlpha | TODO |
| createdAt | The date and time when the file was first uploaded. The format is `YYYY-MM-DDTHH:mm:ss.sssZ` |
| updatedAt | The date and time when the file was last updated. The format is `YYYY-MM-DDTHH:mm:ss.sssZ` |

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

`createdAt >= 7d AND size > 2mb`

{% tabs %}
{% tab title="cURL" %}
```bash
curl -X GET 'https://api.imagekit.io/v1/files?searchQuery="createdAt >= 7d AND size > 2mb"' \
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
    searchQuery : 'createdAt >= 7d AND size > 2mb'
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

list_files = imagekit.list_files({'searchQuery': 'name="createdAt >= 7d AND size > 2mb"'})

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
    "searchQuery" => 'createdAt >= 7d AND size > 2mb',
));

echo ("List files : " . json_encode($listFiles));
```
{% endtab %}

{% tab title="Java" %}
```java
Map<String , String> options=new HashMap<>();
options.put("searchQquery",'createdAt >= 7d AND size > 2mb');
ResultList resultList=ImageKit.getInstance().getFileList(options);
```
{% endtab %}

{% tab title="Ruby" %}
```ruby
imagekitio = ImageKit::ImageKitClient.new("your_private_key", "your_public_key", "your_url_endpoint")
list_files = imagekitio.list_files({searchQuery : 'name="createdAt >= 7d AND size > 2mb"'})
```
{% endtab %}
{% endtabs %}

### Search media library files by name

List all files with filename `file-name.jpg`. We will use the following value of the`searchQuery` parameter. The match is always case-sensitive.

`name="file-name.jpg"`

{% tabs %}
{% tab title="cURL" %}
```bash
curl -X GET 'https://api.imagekit.io/v1/files?searchQuery=name="file-name.jpg"' \
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
curl -X GET 'https://api.imagekit.io/v1/files?searchQuery=format="png"' \
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

