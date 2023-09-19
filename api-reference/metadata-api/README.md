# Metadata API

You can programmatically get image exif, pHash, and other metadata using either of the API below:

1. [Get file metadata from remote URL](get-image-metadata-from-remote-url.md) if you don't want to upload image files to ImageKit.io media library or,
2. [Get file metadata for uploaded media files](get-image-metadata-for-uploaded-media-files.md) if you want to fetch metadata for already uploaded files in your ImageitKit.io media library.

## Metadata Object Structure

### For images
```json
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

### For videos
```json
{
    "height": 720,
    "width": 1280,
    "bitRate ": 546524,
    "duration": 70,
    "audioCodec ": "aac",
    "videoCodec ": "h264",
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

For more information about the Exif standard, please refer to the specification found on [http://www.exif.org](http://www.exif.org). A comprehensive list of available Exif attributes and their meaning can be found on [http://www.sno.phy.queensu.ca/\~phil/exiftool/TagNames/](http://www.sno.phy.queensu.ca/\~phil/exiftool/TagNames/).

### Perceptual Hash (pHash)

Perceptual hashing allows you to construct a hash value that uniquely identifies an input image based on the image's contents. It is different from cryptographic hash functions like MD5 and SHA1. pHash provides similar hash value after minor distortions, like small rotations, blurring, and compression in the image.

[ImageKit.io metadata API](./) returns the pHash value of an image in the metadata response as a hexadecimal string. More information about pHash can be found on [https://www.phash.org/](https://www.phash.org).

### Using pHash to find similar or duplicate images

The hamming distance between two pHash values determines how similar or different the images are.

The pHash value returned by ImageKit.io metadata API is a hexadecimal string of 64bit pHash. The distance between two hash can be between 0 and 64. A lower distance means similar images. If the distance is 0, that means two images are identical. 

To calculate a similarity score between 0 and 1, we can do:

$$
SimilarityScore = 1 - (phashDistance(phash1, phash2) / 64
$$

For example, consider these two images. The first image with pHash value **63433b3ccf8e1ebe**

![pHash = 63433b3ccf8e1ebe](<../../.gitbook/assets/first (1).jpg>)

Second with pHash value **f5d2226cd9d32b16**

![pHash = f5d2226cd9d32b16](<../../.gitbook/assets/second (1).jpg>)

The distance between two pHash values can be calculated using the utility function provided by [ImageKit.io server-side SDKs](../api-introduction/sdk.md#server-side-sdks).

The hamming distance between **63433b3ccf8e1ebe** and **f5d2226cd9d32b16** is **27.**

{% tabs %}
{% tab title="Node.js" %}
```javascript
const ImageKit = require('imagekit');

const imagekit = new ImageKit({
    publicKey : "your_public_api_key",
    privateKey : "your_private_api_key",
    urlEndpoint : "https://ik.imagekit.io/your_imagekit_id/"
});

imagekit.pHashDistance("63433b3ccf8e1ebe", "f5d2226cd9d32b16");
// output: 27
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

# distance is 27
distance = imagekit.phash_distance("63433b3ccf8e1ebe", "f5d2226cd9d32b16"),
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


$distance = $imageKit->pHashDistance("63433b3ccf8e1ebe", "f5d2226cd9d32b16");
echo $distance;
// 27
```
{% endtab %}
{% endtabs %}

$$
SimilarityScore = 1-27/64 = 0.578125
$$

The similarity score is 57%. This means the two images are not similar.

Now let's consider a case of two similar images. The first image with pHash value **63433b3ccf8e1ebe**

![pHash = 63433b3ccf8e1ebe](<../../.gitbook/assets/first (1).jpg>)

Let's resize & crop this image to 300x400 and reduce the quality using aggressive compression. The pHash value of the slightly modified image is **61433b3fcf8f9faf**

![pHash = 61433b3fcf8f9faf](../../.gitbook/assets/first-slightly-different.jpg)

The hamming distance between these two pHash values is **8**

{% tabs %}
{% tab title="Node.js" %}
```javascript
const ImageKit = require('imagekit');

const imagekit = new ImageKit({
    publicKey : "your_public_api_key",
    privateKey : "your_private_api_key",
    urlEndpoint : "https://ik.imagekit.io/your_imagekit_id/"
});

imagekit.pHashDistance("63433b3ccf8e1ebe", "61433b3fcf8f9faf");
// output: 8
```
{% endtab %}
{% endtabs %}

$$
SimilarityScore = 1-8/64 = 0.875
$$

The similarity score is 87%, so it is safe to say that the two images are similar.
