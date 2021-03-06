# Client side file upload

You can upload files to ImageKit.io media library directly from the client-side in Javascript, or Android or iPhone app using [signature-based authentication](client-side-file-upload.md#signature-generation-for-client-side-file-upload). You will need to implement `authenticationEndpoint` endpoint on your backend server as shown [here](client-side-file-upload.md#how-to-implement-authenticationendpoint-endpoint).

You can use ImageKit [client-side SDKs](../api-introduction/sdk.md#client-side-sdks) to get started quickly. See [example usage](client-side-file-upload.md#examples).

{% hint style="info" %}
**File size limit**  
The maximum upload file size is limited to 25MB.
{% endhint %}

## Endpoint

| Method | Endpoint |
| :--- | :--- |
| POST | [https://upload.imagekit.io/api/v1/files/upload](https://upload.imagekit.io/api/v1/files/upload) |

## Request structure \(multipart/form-data\)

<table>
  <thead>
    <tr>
      <th style="text-align:left">Parameter</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><b>file<br /></b>required<b><br /></b>
      </td>
      <td style="text-align:left">
        <p>This field accepts three kinds of values:</p>
        <p></p>
        <ul>
          <li><code>binary</code> - You can send the content of the file as binary. This
            is used when a file is being uploaded from the browser.</li>
          <li><code>base64</code> - Base64 encoded string of file content.</li>
          <li><code>url</code> - URL of the file from where to download the content before
            uploading. Downloading a file from a URL might take longer, so it is recommended
            that you pass the binary or base64 content of the file. Pass the full URL,
            for example - <code>https://www.example.com/rest-of-the-image-path.jpg</code>.</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>publicKey</b>
        <br />required
        <br />
      </td>
      <td style="text-align:left">Your public API key.&#xA0;
        <br /><b><br />Note:</b>&#xA0;This field is only required when uploading the
        file from the client-side. The only purpose of this is to identify your
        account.</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>signature</b>
        <br />required
        <br />
      </td>
      <td style="text-align:left">
        <p>HMAC-SHA1 digest of the&#xA0;<code>token+expire</code>&#xA0;using your
          ImageKit.io <a href="../api-introduction/api-keys.md#private-key">private API key</a>&#xA0;as
          a key. Learn how to create a signature below on the page. This should be
          in lowercase.
          <br />
        </p>
        <p>&#x26A0; <b>Warning:</b>&#xA0;Signature must be calculated on the server-side.
          This field is required for authentication when uploading a file from the
          client-side.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>expire</b>
        <br />required</td>
      <td style="text-align:left">The time until your signature is valid. It must be a <a href="https://en.wikipedia.org/wiki/Unix_time">Unix time</a> in
        less than 1 hour into the future. It should be in seconds.</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>token</b>
        <br />required</td>
      <td style="text-align:left">A unique value generated by the client, which will be used by the ImageKit.io
        server to recognize and prevent subsequent retries for the same request.&#xA0;We
        suggest using V4 UUIDs, or another random string with enough entropy to
        avoid collisions.
        <br />
        <br /><b>Note:</b> Sending a value that has been used in the past will result
        in a validation error. Even if your previous request resulted in an error,
        you should always send a new value for this field.</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>fileName<br /></b>required</td>
      <td style="text-align:left">
        <p>The name with which the file has to be uploaded.</p>
        <p>
          <br />The file name can contain:
          <br />
          <br />- Alphanumeric Characters: <code>a-z</code> , <code>A-Z</code> , <code>0-9</code> (including
          unicode letters, marks, and numerals in other languages)
          <br />- Special Characters: <code>.</code>  <code>_</code> and <code>-</code>
        </p>
        <p></p>
        <p>Any other character including space will be replaced by <code>_</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>useUniqueFileName<br /></b>optional</td>
      <td style="text-align:left">
        <p>Whether to use a unique filename for this file or not.</p>
        <p></p>
        <ul>
          <li>Accepts <code>true</code> or <code>false</code>.</li>
          <li>If set <code>true</code>, ImageKit.io will add a unique suffix to the filename
            parameter to get a unique filename.</li>
          <li>If set <code>false</code>, then the image is uploaded with the provided
            filename parameter, and any existing file with the same name is replaced.</li>
        </ul>
        <p><b>Default value</b> - <code>true</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>tags<br /></b>optional<b><br /></b>
      </td>
      <td style="text-align:left">
        <p>Set the tags while uploading the file.</p>
        <p></p>
        <ul>
          <li>A comma-separated value of tags in the format <code>tag1,tag2,tag3</code>.
            For example - <code>t-shirt,round-neck,men</code>
          </li>
          <li>The maximum length of all characters should not exceed 500.</li>
          <li><code>%</code> is not allowed.</li>
          <li>If this field is not specified and the file is overwritten, then the tags
            will be removed.</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>folder<br /></b>optional<b><br /></b>
      </td>
      <td style="text-align:left">
        <p>The folder path (e.g. <code>/images/folder/</code>) in which the image
          has to be uploaded. If the folder(s) didn&apos;t exist before, a new folder(s)
          is created.<b><br /></b>
        </p>
        <p>The folder name can contain:
          <br />
          <br />- Alphanumeric Characters: <code>a-z</code> , <code>A-Z</code> , <code>0-9</code> (including
          unicode letters, marks, and numerals in other languages)
          <br />- Special Characters: <code>/</code>  <code>_</code> and <code>-</code>
          <br
          />- Using multiple <code>/</code> creates a nested folder.
          <br />
        </p>
        <p><b>Default value</b> - /</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>isPrivateFile<br /></b>optional<b><br /></b>
      </td>
      <td style="text-align:left">
        <p>Whether to mark the file as private or not. This is only relevant for
          image type files</p>
        <p></p>
        <ul>
          <li>Accepts <code>true</code> or <code>false</code>.</li>
          <li>If set <code>true</code>, the file is marked as private which restricts
            access to the original image URL and unnamed image transformations without
            signed URLs. Without the signed URL, only named transformations work on
            private images</li>
        </ul>
        <p><b>Default value</b> - <code>false</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>customCoordinates<br /></b>optional<b><br /></b>
      </td>
      <td style="text-align:left">
        <p>Define an important area in the image. This is only relevant for image
          type files.
          <br />
        </p>
        <ul>
          <li>To be passed as a string with the x and y coordinates of the top-left
            corner, and width and height of the area of interest in the format <code>x,y,width,height</code>.
            For example - <code>10,10,100,100</code>
          </li>
          <li>Can be used with <code>fo-custom</code>transformation.</li>
          <li>If this field is not specified and the file is overwritten, then customCoordinates
            will be removed.</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>responseFields<br /></b>optional
        <br />
      </td>
      <td style="text-align:left">Comma-separated values of the fields that you want ImageKit.io to return
        in the response. For example, set the value of this field to <code>tags,customCoordinates,isPrivateFile,metadata</code> to
        get the value of <code>tags</code>, <code>customCoordinates</code>, <code>isPrivateFile</code> ,
        and <code>metadata</code> in the response.</td>
    </tr>
  </tbody>
</table>

## Response code and structure \(JSON\)

In case of an error, you will get an [error code](../api-introduction/#error-codes) along with the error message. On successful upload, you will receive a `200` status code with uploaded file details in a JSON-encoded response body.

```javascript
{
    "fileId" : "598821f949c0a938d57563bd",
    "name": "file1.jpg",
    "url": "https://ik.imagekit.io/your_imagekit_id/images/products/file1.jpg",
    "thumbnailUrl": "https://ik.imagekit.io/your_imagekit_id/tr:n-media_library_thumbnail/images/products/file1.jpg",
    "height" : 300,
    "width" : 200,
    "size" : 83622,
    "filePath": "/images/products/file1.jpg",
    "tags": ["t-shirt","round-neck","sale2019"],
    "isPrivateFile" : false,
    "customCoordinates" : null,
    "fileType": "image"
}
```

### Understanding response

The JSON-encoded response contains details of the file which can have the following properties:

| Field | Description |
| :--- | :--- |
| fileId | Unique fileId. Store this fileld in your database, as this will be used to perform update action on this file. |
| name | The name of the uploaded file. |
| url | The URL of the file. |
| thumbnailUrl | In the case of an image, a small thumbnail URL. |
| height | Height of the uploaded image file. Only applicable when the file type is an image.  |
| width | Width of the uploaded image file. Only applicable when the file type is an image. |
| size | Size of the uploaded file in bytes. |
| fileType | Type of file. It can either be `image` or `non-image`. |
| filePath  | The path of the file uploaded. It includes any folder that you specified while uploading. |
| tags | Anarray of tags associated with the file. |
| isPrivateFile | Is the file marked as private. It can be either `true` or `false`. |
| customCoordinates | Value of custom coordinates associated with the image in the format `x,y,width,height`. |
| metadata | The metadata of the upload file. Use `fields` property in request to get the metadata returned in the response of upload API. |

## Signature generation for client-side file upload

`signature` is a string sent along with your upload request for authentication when using upload API from the client-side. Generating it requires your ImageKit.io [private API key](../api-introduction/api-keys.md#private-key), and hence this should be generated on your backend. Your backend should ideally implement an API that should provide `signature`.

The `signature` is HMAC-SHA1 digest of the string `token+expire` using your ImageKit.io [private API key](../api-introduction/api-keys.md#private-key) as a key. The `signature` should be in lowercase.

{% hint style="danger" %}
**Never publish your private key on client-side**   
The Private API key should be kept confidential and only stored on your own servers.
{% endhint %}

If you are using ImageKit.io [client-end SDK](../api-introduction/sdk.md#client-side-sdks) for file upload, it requires an `authenticationEndpoint` endpoint for getting authentication parameters required in the upload API.

### How to implement authenticationEndpoint endpoint?

This endpoint is specified by `authenticationEndpoint` parameter during initialization. The SDK makes an HTTP GET request to this endpoint and expects a JSON response with three fields i.e. `signature`, `token` and `expire`.  

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
    private_key='your private_key',
    public_key='your public_key',
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
**Never publish your private key on client-side**   
The Private API key should be kept confidential and only stored on your own servers.
{% endhint %}

## Examples

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

{% tab title="jQuery \(without SDK\)" %}
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
		        if(typeof callback != "function") return;
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

### 

