# Metadata API

You can programmatically get image exif, pHash and other metadata using either of the API below:

1. [Get image metadata from remote URL](get-image-metadata-from-remote-url.md) if you don't want to upload image files to ImageKit.io media library or,
2. [Get image metadata for uploaded media files](get-image-metadata-for-uploaded-media-files.md) if you want to fetch metadata for already uploaded image files in your ImageitKit.io media library.

## Metadata Object Structure

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

### Exif

For more information about the Exif standard please refer to the specification found on [http://www.exif.org](http://www.exif.org/). A comprehensive list of available Exif attributes and their meaning can be found on [http://www.sno.phy.queensu.ca/~phil/exiftool/TagNames/](http://www.sno.phy.queensu.ca/~phil/exiftool/TagNames/).

### Perceptual Hash \(pHash\)

Perceptual hashing allows you to construct a hash value that uniquely identifies an input image based on the contents of an image. [ImageKit.io metadata API](./) returns the pHash value of an image in the metadata response as a hexadecimal string. You can use this value to find a duplicate \(or similar\) image by calculating distance between pHash value of two images.

More information about it can be found on [https://www.phash.org/](https://www.phash.org/).

## Examples

### Calculate pHash distance

Distance between two pHash values can be calculated using utility function provided [ImageKit.io server-side SDKs](../api-introduction/sdk.md#server-side-sdks).

{% tabs %}
{% tab title="Node.js" %}
```javascript
const ImageKit = require('imagekit');

const imagekit = new ImageKit({
    publicKey : "your_public_api_key",
    privateKey : "your_private_api_key",
    urlEndpoint : "https://ik.imagekit.io/your_imagekit_id/"
});

// Some examples:
imagekit.pHashDistance('f06830ca9f1e3e90', 'f06830ca9f1e3e90');
// output: 0 (same image)
imagekit.pHashDistance('2d5ad3936d2e015b', '2d6ed293db36a4fb');
// output: 17 (similar images)
imagekit.pHashDistance('a4a65595ac94518b', '7838873e791f8400');
// output: 37 (dissimilar images)
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

print(
    "Phash distance-", imagekit.phash_distance("f06830ca9f1e3e90", "f06830ca9f1e3e90"),
)
```
{% endtab %}

{% tab title="PHP" %}
```php
$public_key = "your_public_api_key";
$your_private_key = "your_private_api_key";
$url_end_point = "https://ik.imagekit.io/your_imagekit_id";

$imageKit = new ImageKit(
    $public_key,
    $your_private_key,
    $url_end_point
);

$distance = $imageKit->pHashDistance("f06830ca9f1e3e90", "f06830ca9f1e3e90");

echo("Phash Distance : " . $distance);
```
{% endtab %}
{% endtabs %}

