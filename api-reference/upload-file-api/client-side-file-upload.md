# Client side file upload

You can upload files to the ImageKit.io media library directly from the client-side in Javascript, or Android or iPhone app using [signature-based authentication](client-side-file-upload.md#signature-generation-for-client-side-file-upload). You will need to implement `authenticationEndpoint` endpoint on your backend server as shown [here](client-side-file-upload.md#how-to-implement-authenticationendpoint-endpoint).

You can use ImageKit [client-side SDKs](../api-introduction/sdk.md#client-side-sdks) to get started quickly. See [example usage](client-side-file-upload.md#examples).

{% hint style="info" %}
**File size limit**\
The maximum upload file size is limited to 25MB on the free plan. On paid plan, this limit is 300MB for video files.

**Version limit**\
A file can have a maximum of 100 versions.
{% endhint %}

## Endpoint

| Method | Endpoint                                                                                         |
| ------ | ------------------------------------------------------------------------------------------------ |
| POST   | [https://upload.imagekit.io/api/v1/files/upload](https://upload.imagekit.io/api/v1/files/upload) |

## Request structure (multipart/form-data)

| Parameter                                                                                   | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| ------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p><strong>file</strong><br><strong></strong>required<br><strong></strong></p>              | <p>This field accepts three kinds of values:</p><p></p><ul><li><code>binary</code> - You can send the content of the file as binary. This is used when a file is being uploaded from the browser.</li><li><code>base64</code> - Base64 encoded string of file content.</li><li><code>url</code> - URL of the file from where to download the content before uploading. For example - <code>https://www.example.com/rest-of-the-image-path.jpg</code>.</li></ul><p><strong>Note:</strong> When passing a URL in the file parameter, please ensure that our servers can access the URL. In case ImageKit is unable to download the file from the specified URL, a <code>400</code> error response is returned. In addition to this, the file download request is aborted if response headers are not received in 8 seconds. This will also result in a <code>400</code> error. </p> |
| <p><strong>publicKey</strong><br>required<br></p>                                           | <p>Your public API key. <br><br><strong>Note:</strong> This field is only required when uploading the file from the client-side. The only purpose of this is to identify your account.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| <p><strong>signature</strong><br>required<br></p>                                           | <p>HMAC-SHA1 digest of the <code>token+expire</code> using your ImageKit.io <a href="../api-introduction/api-keys.md#private-key">private API key</a> as a key. Learn how to create a signature below on the page. This should be in lowercase.<br></p><p><span data-gb-custom-inline data-tag="emoji" data-code="26a0">⚠</span> <strong>Warning:</strong> Signature must be calculated on the server-side. This field is required for authentication when uploading a file from the client-side.</p>                                                                                                                                                                                                                                                                                                                                                                             |
| <p><strong>expire</strong><br>required</p>                                                  | The time until your signature is valid. It must be a [Unix time](https://en.wikipedia.org/wiki/Unix\_time) in less than 1 hour into the future. It should be in seconds.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| <p><strong>token</strong><br>required</p>                                                   | <p>A unique value generated by the client, which will be used by the ImageKit.io server to recognize and prevent subsequent retries for the same request. We suggest using V4 UUIDs, or another random string with enough entropy to avoid collisions.<br><br><strong>Note:</strong> Sending a value that has been used in the past will result in a validation error. Even if your previous request resulted in an error, you should always send a new value for this field.</p>                                                                                                                                                                                                                                                                                                                                                                                                 |
| <p><strong>fileName</strong><br><strong></strong>required</p>                               | <p>The name with which the file has to be uploaded. </p><p><br>The file name can contain:<br><br>- Alphanumeric Characters: <code>a-z</code> , <code>A-Z</code> , <code>0-9</code> (including unicode letters, marks, and numerals in other languages)<br>- Special Characters: <code>.</code> <code>_</code> and <code>-</code></p><p></p><p>Any other character including space will be replaced by <code>_</code></p>                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| <p><strong>useUniqueFileName</strong><br><strong></strong>optional</p>                      | <p>Whether to use a unique filename for this file or not.</p><p></p><ul><li>Accepts <code>true</code> or <code>false</code>.</li><li>If set <code>true</code>, ImageKit.io will add a unique suffix to the filename parameter to get a unique filename.</li><li>If set <code>false</code>, then the image is uploaded with the provided filename parameter, and any existing file with the same name is replaced.</li></ul><p><strong>Default value</strong> - <code>true</code></p>                                                                                                                                                                                                                                                                                                                                                                                              |
| <p><strong>tags</strong><br><strong></strong>optional<br><strong></strong></p>              | <p>Set the tags while uploading the file.</p><p></p><ul><li>A comma-separated value of tags in the format <code>tag1,tag2,tag3</code>. For example - <code>t-shirt,round-neck,men</code></li><li>The maximum length of all characters should not exceed 500.</li><li><code>%</code> is not allowed.</li><li>If this field is not specified and the file is overwritten, then the tags will be removed.</li></ul>                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| <p><strong>folder</strong><br><strong></strong>optional<br><strong></strong></p>            | <p>The folder path (e.g. <code>/images/folder/</code>) in which the image has to be uploaded. If the folder(s) didn't exist before, a new folder(s) is created. The nesting of folders can be at most 50 levels deep.<br><strong></strong></p><p>The folder name can contain:<br><br>- Alphanumeric Characters: <code>a-z</code> , <code>A-Z</code> , <code>0-9</code> (including unicode letters, marks, and numerals in other languages)<br>- Special Characters: <code>/</code> <code>_</code> and <code>-</code><br>- Using multiple <code>/</code> creates a nested folder.<br></p><p><strong>Default value</strong> - /</p>                                                                                                                                                                                                                                                                                                       |
| <p><strong>isPrivateFile</strong><br><strong></strong>optional<br><strong></strong></p>     | <p>Whether to mark the file as private or not. This is only relevant for image type files</p><p></p><ul><li>Accepts <code>true</code> or <code>false</code>.</li><li>If set <code>true</code>, the file is marked as private which restricts access to the original image URL and unnamed image transformations without signed URLs. Without the signed URL, only named transformations work on private images</li></ul><p><strong>Default value</strong> - <code>false</code></p>                                                                                                                                                                                                                                                                                                                                                                                                |
| <p><strong>isPublished</strong><br>optional<br></p>          | <p>Whether to upload file as published or not.<br><br>- Accepts <code>true</code> or <code>false</code>.<br>- If set <code>false</code>, the file is marked as unpublished, which restricts access to the file only via the media library. Files in [draft or unpublished](../../media-library/overview/draft-assets.md) state can only be publicly accessed after being published.<br>- The option to upload in draft state is only available in custom enterprise pricing plans.<br><br><strong>Default value</strong> - <code>true</code></p>                                                                                                                                                                                                                                                                                                                                                                                              |
| <p><strong>customCoordinates</strong><br><strong></strong>optional<br><strong></strong></p> | <p>Define an important area in the image. This is only relevant for image type files.<br></p><ul><li>To be passed as a string with the x and y coordinates of the top-left corner, and width and height of the area of interest in the format <code>x,y,width,height</code>. For example - <code>10,10,100,100</code></li><li>Can be used with <code>fo-custom</code>transformation.</li><li>If this field is not specified and the file is overwritten, then customCoordinates will be removed.</li></ul>                                                                                                                                                                                                                                                                                                                                                                        |
| <p><strong>responseFields</strong><br><strong></strong>optional<br></p>                     | Comma-separated values of the fields that you want the API to return in the response. For example, set the value of this field to `tags,customCoordinates,isPrivateFile` to get the value of `tags`, `customCoordinates`, and `isPrivateFile` in the response. Accepts combination of `tags`, `customCoordinates`, `isPrivateFile`, `embeddedMetadata`, `customMetadata` and `metadata`.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| <p><strong>extensions</strong></p><p>optional</p>                                           | <p>Stringified JSON object with array of extensions to be processed on the image.</p><p></p><p>For reference about extensions <a href="../../extensions/overview/">read here</a>.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| <p><strong>webhookUrl</strong></p><p>optional</p>                                           | Final status of pending extensions will be sent to this URL. To learn more about how ImageKit uses webhooks, [refer here](../../extensions/overview/#webhooks).                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| <p><strong>overwriteFile</strong></p><p>optional</p>                                        | Default is `true`. If `overwriteFile` is set to `false` and `useUniqueFileName` is also `false`, and a file already exists at the exact location, upload API will return an error immediately.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| <p><strong>overwriteAITags</strong></p><p>optional</p>                                      | Default is `true`. If set to `true` and a file already exists at the exact location, its `AITags` will be removed. Set `overwriteAITags` to `false` to preserve `AITags`.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| <p><strong>overwriteTags</strong></p><p>optional</p>                                        | Default is `true`. If the request does not have `tags` ,  `overwriteTags` is set to `true` and a file already exists at the exact location, exiting `tags` will be removed. In case the request body has `tags`, setting `overwriteTags` to `false` has no effect and request's `tags` are set on the asset.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| <p><strong>overwriteCustomMetadata</strong></p><p>optional</p>                              | Default is `true`. If the request does not have `customMetadata` ,  `overwriteCustomMetadata` is set to `true` and a file already exists at the exact location, exiting `customMetadata` will be removed. In case the request body has `customMetadata`, setting `overwriteCustomMetadata` to `false` has no effect and request's `customMetadata` is set on the asset.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| <p><strong>customMetadata</strong></p><p>optional</p>                                         |  Stringified JSON key-value data to be associated with the asset. Checkout `overwriteCustomMetadata` parameter to understand default behaviour. Before setting any custom metadata on an asset you have to create the field using [custom metadata fields API](../custom-metadata-fields-api/).                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |

## Response code and structure (JSON)

In case of an error, you will get an [error code](../api-introduction/#error-codes) along with the error message. On successful upload, you will receive a `200` status code with uploaded file details in a JSON-encoded response body.

```javascript
{
    "fileId": "598821f949c0a938d57563bd",
    "name": "file1.jpg",
    "url": "https://ik.imagekit.io/your_imagekit_id/images/products/file1.jpg",
    "thumbnailUrl": "https://ik.imagekit.io/your_imagekit_id/tr:n-media_library_thumbnail/images/products/file1.jpg",
    "height": 300,
    "width": 200,
    "size": 83622,
    "filePath": "/images/products/file1.jpg",
    "tags": ["t-shirt", "round-neck", "sale2019"],
    "AITags": [
        {
            "name": "Shirt",
            "confidence": 90.12,
            "source": "google-auto-tagging"
        },
        /* ... more googleVision tags ... */
    ],
    "versionInfo": {
            "id": "598821f949c0a938d57563bd",
            "name": "Version 1"
    },
    "isPrivateFile": false,
    "customCoordinates": null,
    "customMetadata": {
        brand: "Nike",
        color: "red"
    },
    "embeddedMetadata": {
        Title: "The Title (ref2019.1)",
        Description: "The description aka caption (ref2019.1)",
        State: "Province/State(Core)(ref2019.1)",
        Copyright: "Copyright (Notice) 2019.1 IPTC - www.iptc.org  (ref2019.1)"
    },
    "extensionStatus": {
        "google-auto-tagging": "success",
        "aws-auto-tagging": "pending"
    },
    "fileType": "image"
}
```

### Understanding response

The JSON-encoded response contains details of the file which can have the following properties:

| Property name     | Description                                                                                                                                                                                                                             |
| ----------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| fileId            | Unique `fileId`. Store this `fileld` in your database, as this will be used to perform update action on this file                                                                                                                       |
| name              | Name of the file.                                                                                                                                                                                                                       |
| filePath          | The relative path of the file. In the case of an image, you can use this path to construct different transformations.                                                                                                                   |
| tags              | The array of tags associated with the image. If no tags are set, it will be `null`.                                                                                                                                                     |
| AITags            | Array of `AITags` associated with the image. If no `AITags` are set, it will be `null`. These tags can be added using the `google-auto-tagging` or `aws-auto-tagging` [extensions](../../extensions/overview/ai-based-auto-tagging.md). |
| versionInfo       | An object containing the file or file version's <code>id</code> (versionId) and <code>name</code>. |
| isPrivateFile     | Is the file marked as private. It can be either `true` or `false`.                                                                                                                                                                      |
| customCoordinates | <p>Value of custom coordinates associated with the image in the format <code>x,y,width,height</code>. If customCoordinates are not defined, then it is <code>null</code>.<br></p>                                                       |
| url               | A publicly accessible URL of the file.                                                                                                                                                                                                  |
| thumbnailUrl      | In the case of an image, a small thumbnail URL.                                                                                                                                                                                         |
| fileType          | The type of file could be either `image` or `non-image`.                                                                                                                                                                                |
| height            | Height of the media file in pixels (Only for images and videos)                                                                                                                                                                         |
| width             | Width of the media file in pixels (Only for images and videos)                                                                                                                                                                          |
| size              | Size of the file in Bytes                                                                                                                                                                                                               |
| bitRate           | Bitrate of the media file (Only for videos)                                                                                                                                                                                             |
| videoCodec        | Video codec of the first stream for the media file (Only for videos)                                                                                                                                                                    |
| audioCodec        | Audio codec of the first stream for the media file (Only for videos)                                                                                                                                                                    |
| duration          | Duration of the media file content in seconds (Only for videos)                                                                                                                                                                         |
| customMetadata    | A key-value data associated with the asset. Use `responseField` in API request to get `customMetadata` in the upload API response. Before setting any custom metadata on an asset, you have to create the field using [custom metadata fields API](../custom-metadata-fields-api/).                                                                                                                                                                                                                         |
| embeddedMetadata  | Consolidated embedded metadata associated with the file. It includes `exif`, `iptc`, and `xmp` data. Use `responseField` in API request to get `embeddedMetadata` in the upload API response.                                           |
| metadata          | Metadata associated with the file in legacy format.                                                                                                                                                                                     |
| extensionStatus   | <p>Extension names with their processing status at the time of completion of the request. It could have one of the following status values:</p><ul><li><code>success</code>: The extension has been successfully applied.</li><li><code>failed</code>: The extension has failed and will not be retried.</li><li><code>pending</code>: The extension will finish processing in some time. On completion, the final status (success / failed) will be sent to the <code>webhookUrl</code> provided.</li></ul><p>If no extension was requested, then this parameter is not returned.</p>                                                                                                                                                                 |

## Signature generation for client-side file upload

`signature` is a string sent along with your upload request for authentication when using upload API from the client-side. Generating it requires your ImageKit.io [private API key](../api-introduction/api-keys.md#private-key), and hence this should be generated on your backend. Your backend should ideally implement an API that should provide `signature`.

The `signature` is HMAC-SHA1 digest of the string `token+expire` using your ImageKit.io [private API key](../api-introduction/api-keys.md#private-key) as a key. The `signature` should be in lowercase.

{% hint style="danger" %}
**Never publish your private key on client-side**\
The Private API key should be kept confidential and only stored on your own servers.
{% endhint %}

If you are using ImageKit.io [client-end SDK](../api-introduction/sdk.md#client-side-sdks) for file upload, it requires an `authenticationEndpoint` endpoint for getting authentication parameters required in the upload API.

### How to implement authenticationEndpoint endpoint?

This endpoint is specified by `authenticationEndpoint` parameter during initialization. The SDK makes an HTTP GET request to this endpoint and expects a JSON response with three fields i.e. `signature`, `token` and `expire`. &#x20;

Example response:

```javascript
{
    token: "1bab386f-45ea-49e1-9f0d-6afe49a5b250",
    expire: 1580372696,
    signature: "0f9d5a45e97c24fa9200a9d5543c9af1e2c45a54"
}
```

Since calculating these parameters requires your ImageKit.io [private API key](../api-introduction/api-keys.md#private-key), hence this endpoint has to be implemented on your server-side. You can use utility functions provided in all [server-side SDKs](../api-introduction/sdk.md#server-side-sdks) to implement this endpoint as shown below.

{% tabs %}
{% tab title="Pseudo code" %}
```javascript
var token = req.query.token || uuid.v4();
var expire = req.query.expire || parseInt(Date.now()/1000)+2400;
var privateAPIKey = "your_private_key";
var signature = crypto.createHmac('sha1', privateAPIKey).update(token+expire).digest('hex');
res.set({
    "Access-Control-Allow-Origin" : "*"
})
res.status(200);
res.send({
    token : token,
    expire : expire,
    signature : signature
})
```
{% endtab %}

{% tab title="Node.js" %}
```javascript
var ImageKit = require("imagekit");
var fs = require('fs');

var imagekit = new ImageKit({
    publicKey : "your_public_api_key",
    privateKey : "your_private_api_key",
    urlEndpoint : "https://ik.imagekit.io/your_imagekit_id/"
});

var authenticationParameters = imagekit.getAuthenticationParameters();
console.log(authenticationParameters);
```
{% endtab %}

{% tab title="Python" %}
```python
import base64
import os
import sys
from imagekitio import ImageKit

imagekit = ImageKit(
    public_key='your public_key',
    private_key='your private_key',
    url_endpoint = 'your url_endpoint'
)

auth_params = imagekit.get_authentication_parameters()

print("Auth params-", auth_params)
```
{% endtab %}

{% tab title="PHP" %}
```php
use ImageKit\ImageKit;

$public_key = "your_public_key";
$your_private_key = "your_private_key";
$url_end_point = "https://ik.imagekit.io/your_imagekit_id";
$sample_file_path = "/sample.jpg";

$imageKit = new ImageKit(
    $public_key,
    $your_private_key,
    $url_end_point
);

$authenticationParameters = $imageKit->getAuthenticationParameters();

echo("Auth params : " . json_encode($authenticationParameters));
```
{% endtab %}
{% endtabs %}

{% hint style="danger" %}
**Never publish your private key on client-side**\
The Private API key should be kept confidential and only stored on your own servers.
{% endhint %}

## Examples

The example below demonstrates only basic usage. Refer to [these examples](server-side-file-upload.md#examples) in the server-side upload section to learn about different use-cases. The only difference between client-side and server-side upload is how API authentication works.

Make sure you have implemented `authenticationEndpoint` endpoint on your server as shown [here](client-side-file-upload.md#how-to-implement-authenticationendpoint-endpoint) before using the below examples.

{% tabs %}
{% tab title="JavaScipt SDK" %}
{% code title="index.html" %}
```markup
<form action="#" onsubmit="upload()">
	<input type="file" id="file1" />
	<input type="submit" />
</form>
<script type="text/javascript" src="../dist/imagekit.js"></script>

<script>
    /* 
        SDK initilization
        
        authenticationEndpoint should be implemented on your server 
        as shown above 
    */
    var imagekit = new ImageKit({
        publicKey : "your_public_api_key",
        urlEndpoint : "https://ik.imagekit.io/your_imagekit_id",
        authenticationEndpoint : "https://www.yourserver.com/auth"
    });
    
    // Upload function internally uses the ImageKit.io javascript SDK
    function upload(data) {
        var file = document.getElementById("file1");
        imagekit.upload({
            file : file.files[0],
            fileName : "abc.jpg",
            tags : ["tag1"]
        }, function(err, result) {
            console.log(arguments);
            console.log(imagekit.url({
                src: result.url,
                transformation : [{ height: 300, width: 400}]
            }));
        })
    }
</script>
```
{% endcode %}
{% endtab %}

{% tab title="jQuery (without SDK)" %}
```markup
<form action="#" onsubmit="upload()">
	<input type="file" id="file1" />
	<input type="submit" />
</form>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.4.1/jquery.min.js" integrity="sha256-CSXorXvZcTkaix6Yvo6HppcZGetbYMGWSFlBw8HfCJo=" crossorigin="anonymous"></script>

<script>
	// This endpoint should be implemented on your server as shown above 
	var authenticationEndpoint = "https://www.yourserver.com/auth";
	
	function upload() {
	  var file = document.getElementById("file1");
		var formData = new FormData();
		formData.append("file", file.files[0]);
		formData.append("fileName", "abc.jpg");
		formData.append("publicKey", "your_public_api_key");
	
		// Let's get the signature, token and expire from server side
		$.ajax({
		    url : authenticationEndpoint,
		    method : "GET",
		    dataType : "json",
		    success : function(body) {
		        formData.append("signature", body.signature || "");
		        formData.append("expire", body.expire || 0);
		        formData.append("token", body.token);
	
						// Now call ImageKit.io upload API
		        $.ajax({
		            url : "https://upload.imagekit.io/api/v1/files/upload",
		            method : "POST",
		            mimeType : "multipart/form-data",
		            dataType : "json",
		            data : formData,
		            processData : false,
		            contentType : false,
		            error : function(jqxhr, text, error) {
		                console.log(error)
		            },
		            success : function(body) {
		                console.log(body)
		            }
		        });
	
		    },
	
		    error : function(jqxhr, text, error) {
		        console.log(arguments);
		    }
		});
	}
</script>
```
{% endtab %}

{% tab title="React SDK" %}
```javascript
import React from 'react';
import { IKImage, IKContext, IKUpload } from 'imagekitio-react'

function App() {
  const publicKey = "your_public_api_key";
  let urlEndpoint = "https://ik.imagekit.io/your_imagekit_id";
  const authenticationEndpoint = "https://www.yourserver.com/auth";

  return (
    <div className="App">
      <p>To use this funtionality please remember to setup the server</p>
      <IKContext publicKey={publicKey} urlEndpoint={urlEndpoint} authenticationEndpoint={authenticationEndpoint} >
        <IKUpload fileName="abc.jpg" tags={["tag1"]} useUniqueFileName={true} isPrivateFile= {false} />
      </IKContext>   
    </div>
  );
}

export default App;
```
{% endtab %}

{% tab title="Vue.js SDK" %}
```javascript
<template>
  <div class="sample-app">
    <p>Upload</p>
    <IKContext
      :publicKey="publicKey"
      :urlEndpoint="urlEndpoint"
      :authenticationEndpoint="authenticationEndpoint"
    >
      <IKUpload fileName="abc.jpg" v-bind:tags="['tag1']" v-bind:responseFields="['tags']"/>
    </IKContext>
    <p>To use this funtionality please remember to setup the server</p>
  </div>
</template>

<script>
import { IKImage, IKContext, IKUpload } from "imagekitio-vue";
let urlEndpoint= "https://ik.imagekit.io/your_imagekit_id";

export default {
  name: "app",
  components: {
    IKImage,
    IKContext,
    IKUpload
  },
  data() {
    return {
      urlEndpoint: "https://ik.imagekit.io/your_imagekit_id",
      publicKey: "your_public_api_key",
      authenticationEndpoint: "https://www.yourserver.com/auth"
    };
  }
};
</script>
```
{% endtab %}
{% endtabs %}
