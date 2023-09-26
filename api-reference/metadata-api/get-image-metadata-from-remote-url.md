# Get file metadata from remote URL

{% swagger baseUrl="https://api.imagekit.io" path="/v1/metadata" method="get" summary="Get file metadata from remote URL API" %}
{% swagger-description %}
Get image EXIF, pHash, and other metadata from ImageKit.io powered remote URL using this API.
{% endswagger-description %}

{% swagger-parameter in="header" name="Authorization" type="string" %}
base64 encoding of `your_private_api_key:`

**Note the colon in the end.**
{% endswagger-parameter %}

{% swagger-parameter in="query" name="url" type="string" %}
The encoded URL of the image.

**Note:** This URL should be accessible using your ImageKit.io account.
{% endswagger-parameter %}

{% swagger-response status="200" description="On success, you will receive the file metadata in JSON-encoded response body." %}
```javascript
{
    "height": 68,
    "width": 100,
    "size": 7749,
    "format": "jpg",
    "hasColorProfile": true,
    "quality": 0,
    "density": 72,
    "hasTransparency": false,
	"pHash": "f06830ca9f1e3e90",
    "exif": {
        "image": {
            "Make": "Canon",
            "Model": "Canon EOS 40D",
            "Orientation": 1,
            "XResolution": 72,
            "YResolution": 72,
            "ResolutionUnit": 2,
            "Software": "GIMP 2.4.5",
            "ModifyDate": "2008:07:31 10:38:11",
            "YCbCrPositioning": 2,
            "ExifOffset": 214,
            "GPSInfo": 978
        },
        "thumbnail": {
            "Compression": 6,
            "XResolution": 72,
            "YResolution": 72,
            "ResolutionUnit": 2,
            "ThumbnailOffset": 1090,
            "ThumbnailLength": 1378
        },
        "exif": {
            "ExposureTime": 0.00625,
            "FNumber": 7.1,
            "ExposureProgram": 1,
            "ISO": 100,
            "ExifVersion": "0221",
            "DateTimeOriginal": "2008:05:30 15:56:01",
            "CreateDate": "2008:05:30 15:56:01",
            "ShutterSpeedValue": 7.375,
            "ApertureValue": 5.625,
            "ExposureCompensation": 0,
            "MeteringMode": 5,
            "Flash": 9,
            "FocalLength": 135,
            "SubSecTime": "00",
            "SubSecTimeOriginal": "00",
            "SubSecTimeDigitized": "00",
            "FlashpixVersion": "0100",
            "ColorSpace": 1,
            "ExifImageWidth": 100,
            "ExifImageHeight": 68,
            "InteropOffset": 948,
            "FocalPlaneXResolution": 4438.356164383562,
            "FocalPlaneYResolution": 4445.969125214408,
            "FocalPlaneResolutionUnit": 2,
            "CustomRendered": 0,
            "ExposureMode": 1,
            "WhiteBalance": 0,
            "SceneCaptureType": 0
        },
        "gps": {
            "GPSVersionID": [
                2,
                2,
                0,
                0
            ]
        },
        "interoperability": {
            "InteropIndex": "R98",
            "InteropVersion": "0100"
        },
        "makernote": {}
    }
}
```
{% endswagger-response %}
{% endswagger %}

### Response structure and status code (application/JSON)

In case of error, you will get an [error code](../api-introduction/#error-codes) along with the error message. On success, you will receive a `200` status code with the image metadata in the JSON-encoded response body.

A metadata object example can be found [here](./#metadata-object-structure).

### Metadata through ImageKit transformation URL

If the `url` passed to this API has no transformation parameter, then the metadata of the original uploaded image will be fetched. We internally add [`orig-true`](../../features/image-transformations/resize-crop-and-other-transformations.md#original-image-orig) parameter to fetch the original image.

Instead, if the passed `url` has any transformation parameters, then the metadata of the transformed image will be fetched.

See the [examples](./#examples) below.

## Examples

Here are some example requests to understand the API usage.

### Get metadata of an original image (no transformation applied)

{% tabs %}
{% tab title="cURL" %}
```bash
# The unique fileId of the uploaded file. fileId is returned in response of list files API and upload API.
curl -X GET "https://api.imagekit.io/v1/metadata?url=https://ik.imagekit.io/demo/default-image.jpg" \
-u your_private_api_key:
```
{% endtab %}

{% tab title="Python" %}
```python
get_metadata = imagekit.get_remote_file_url_metadata(remote_file_url="remote_file_url")

print(get_metadata, end="\n\n")

# Raw Response
print(get_metadata.response_metadata.raw)

# print the file metadata fields
print(get_metadata.width)
print(get_metadata.exif.image.x_resolution)
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

$fileMetadata = $imageKit->getFileMetadataFromRemoteURL("https://ik.imagekit.io/demo/tr:w-100/default-image.jpg")

echo("File metadata : " . json_encode($fileMetadata));
```
{% endtab %}

{% tab title="Java" %}
```java
ResultMetaData result=ImageKit.getInstance().getRemoteFileMetadata("remote_file_url");
```
{% endtab %}

{% tab title="Ruby" %}
```ruby
imagekitio = ImageKit::ImageKitClient.new("your_private_key", "your_public_key", "your_url_endpoint")
imagekitio.get_remote_file_url_metadata("remote_file_url")
```
{% endtab %}

{% tab title="Go" %}
```go
resp, err := ik.Metadata.FromUrl(ctx, "remote_file_url")
```
{% endtab %}
{% endtabs %}

### Get metadata of a transformed image

{% tabs %}
{% tab title="cURL" %}
```bash
# The URL of the uploaded file
curl -X GET "https://api.imagekit.io/v1/metadata?url=https://ik.imagekit.io/demo/tr:w-100/default-image.jpg" \
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

imagekit.getFileMetadata("https://ik.imagekit.io/demo/tr:w-100/default-image.jpg", function(error, result) {
    if(error) console.log(error);
    else console.log(result);
});
```
{% endtab %}

{% tab title="Python" %}
```python
get_metadata = imagekit.get_remote_file_url_metadata(remote_file_url="https://ik.imagekit.io/demo/tr:w-100/default-image.jpg")

print(get_metadata, end="\n\n")

# Raw Response
print(get_metadata.response_metadata.raw)

# print the file metadata fields
print(get_metadata.width)
print(get_metadata.exif.image.x_resolution)
```
{% endtab %}

{% tab title="PHP" %}
```php
$imageKit->getFileMetadataFromRemoteURL("https://ik.imagekit.io/demo/tr:w-100/default-image.jpg")
```
{% endtab %}

{% tab title="Java" %}
```java
ResultMetaData result=ImageKit.getInstance().getRemoteFileMetadata("https://ik.imagekit.io/demo/tr:w-100/default-image.jpg");
```
{% endtab %}

{% tab title="Ruby" %}
```ruby
imagekitio = ImageKitIo::Client.new("your_private_key", "your_public_key", "your_url_endpoint")
imagekitio.get_remote_file_url_metadata(remote_file_url: "https://ik.imagekit.io/demo/tr:w-100/default-image.jpg")
```
{% endtab %}

{% tab title="Go" %}
```go
resp, err := ik.Metadata.FromUrl(ctx, "https://ik.imagekit.io/demo/tr:w-100/default-image.jpg")

```
{% endtab %}

{% tab title=".Net" %}
```.net
var imagekit = new ImageKit({
    publicKey : "your_public_api_key",
    privateKey : "your_private_api_key",
    urlEndpoint : "https://ik.imagekit.io/your_imagekit_id/"
});
ResultMetaData resultMetaData1 = imagekit.GetRemoteFileMetadata("https://ik.imagekit.io/demo/tr:w-100/default-image.jpg");
```
{% endtab %}

{% endtabs %}

### Calculate pHash distance between two images

See examples [here](./#calculate-phash-distance).
