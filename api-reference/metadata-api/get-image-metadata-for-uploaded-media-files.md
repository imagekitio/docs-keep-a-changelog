# Get file metadata for uploaded media files

You can programmatically get image EXIF, pHash, and other metadata for uploaded files in the ImageKit.io media library using this API.

{% hint style="info" %}
:bulb: You can also get the file metadata while uploading the file by passing metadata in `responseFields` parameter.
{% endhint %}

{% swagger baseUrl="https://api.imagekit.io" path="/v1/files/:fileId/metadata" method="get" summary="Get file metadata for uploaded media files API" %}
{% swagger-description %}
Get image EXIF, pHash, and other metadata for uploaded files in ImageKit.io media library using this API.
{% endswagger-description %}

{% swagger-parameter in="path" name="fileId" type="string" %}
The unique fileId of the uploaded file. fileId is returned in list files API and upload API.
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authorization" type="string" %}
base64 encoding of `your_private_api_key:`

**Note the colon in the end.**
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

## Examples

Here are some example requests to understand the API usage.

### Get metadata of an uploaded file

{% tabs %}
{% tab title="cURL" %}
```bash
# The unique fileId of the uploaded file. fileId is returned in response of list files API and upload API.
curl -X GET "https://api.imagekit.io/v1/files/file_id/metadata" \
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

imagekit.getFileMetadata("file_id", function(error, result) {
    if(error) console.log(error);
    else console.log(result);
});
```
{% endtab %}

{% tab title="Python" %}
```python
from imagekitio import ImageKit

imagekit = ImageKit(
    public_key='your_public_api_key',
    private_key='your_private_api_key',
    url_endpoint = 'https://ik.imagekit.io/your_imagekit_id/'
)

metadata = imagekit.get_metadata(file_id="file_id")

print("File detail-", metadata, end="\n\n")

# Raw Response
print(metadata.response_metadata.raw)

# print the file metadata fields
print(metadata.width)
print(metadata.exif.image.x_resolution)

# Raw Response
print(metadata.response_metadata.raw)

# print the file metadata fields
print(metadata.width)
print(metadata.exif.image.x_resolution)
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

$fileId = 'file_id';

$fileMetadata = $imageKit->getFileMetaData($fileId);

echo("File metadata : " . json_encode($fileMetadata));
```
{% endtab %}

{% tab title="Java" %}
```java
ResultMetaData result=ImageKit.getInstance().getFileMetadata("file_id");
```
{% endtab %}

{% tab title="Ruby" %}
```ruby
imagekitio = ImageKitIo::Client.new("your_private_key", "your_public_key", "your_url_endpoint")
file_metadata = imagekitio.get_file_metadata(file_id: "file_id")
```
{% endtab %}

{% tab title="Go" %}
```go
resp, err := ik.Metadata.FromFile(ctx, "file_id")
```
{% endtab %}

{% tab title=".Net" %}
```.net
var imagekit = new ImageKit({
    publicKey : "your_public_api_key",
    privateKey : "your_private_api_key",
    urlEndpoint : "https://ik.imagekit.io/your_imagekit_id/"
});
ResultMetaData resultMetaData = imagekit.GetFileMetadata("file_id");
```
{% endtab %}

{% endtabs %}

### Calculate pHash distance between two images

See examples [here](./#calculate-phash-distance).
