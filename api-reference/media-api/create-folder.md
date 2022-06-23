# Create folder

{% swagger baseUrl="https://api.imagekit.io" path="/v1/folder/" method="post" summary="Create folder API" %}
{% swagger-description %}
This will create a new folder. You can specify the folder name and location of the parent folder where this new folder should be created. 
{% endswagger-description %}

{% swagger-parameter in="header" name="Authorization" type="string" %}
base64 encoding of `your_private_api_key:`

**Note the colon in the end.**
{% endswagger-parameter %}

{% swagger-parameter in="query" name="folderName" type="string" %}
The folder will be created with this name. All characters except alphabets and numbers (inclusive of unicode letters, marks, and numerals in other languages) will be replaced by an underscore i.e. `_`.
{% endswagger-parameter %}

{% swagger-parameter in="query" name="parentFolderPath" type="string" %}
The folder where the new folder should be created, for root use `/` else the path e.g. `containing/folder/`.

**Note:** If any folder(s) is not present in the `parentFolderPath` parameter, it will be automatically created. For example, if you pass `/product/images/summer`, then `product`, `images`, and `summer` folders will be created if they don't already exist.
{% endswagger-parameter %}

{% swagger-response status="201" description="Folder created successfully" %}
```
```
{% endswagger-response %}
{% endswagger %}

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
{% tab title="Ruby" %}
```ruby
imagekitio = ImageKitIo::Client.new("your_private_key", "your_public_key", "your_url_endpoint")
imagekitio.create_folder(folder_name: 'new_folder', parent_folder_path: 'source/folder/path')
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

$folderName = 'new_folder';
$parentFolderPath = 'source/folder/path';
$createFolder = $imageKit->createFolder([
    'folderName' => $folderName,
    'parentFolderPath' => $parentFolderPath,
]);

echo("Create Folder : " . json_encode($createFolder));
```
{% endtab %}

{% endtabs %}
