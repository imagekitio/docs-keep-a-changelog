# Secure client side file upload (beta)

You can upload files to the ImageKit.io media library directly from the client-side in Javascript or any client side application using [JSON Web Token (JWT) authentication](secure-client-side-file-upload.md#json-web-token-jwt-generation-for-client-side-file-upload). You will need to implement `authenticationEndpoint` endpoint on your backend server as shown [here](secure-client-side-file-upload.md#how-to-implement-authenticationendpoint-endpoint).

You can use ImageKit's [JavaScript](https://github.com/imagekit-developer/imagekit-javascript) to get started quickly. See [example usage](secure-client-side-file-upload.md#examples).

{% hint style="danger" %}
This API is in beta and subject to change.
{% endhint %}

{% hint style="info" %}
**File size limit**\
The maximum upload file size is limited to 25MB on the free plan. On paid plan, this limit is 300MB for video files.

**Version limit**\
A file can have a maximum of 100 versions.
{% endhint %}

## Endpoint

| Method | Endpoint                                                                                         |
| ------ | ------------------------------------------------------------------------------------------------ |
| POST   | [https://upload.imagekit.io/api/v2/files/upload](https://upload.imagekit.io/api/v2/files/upload) |

## Request structure (multipart/form-data)

| Parameter                                                                                   | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| ------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p><strong>file</strong><br><strong></strong>required<br><strong></strong></p>              | <p>This field accepts three kinds of values:</p><p></p><ul><li><code>binary</code> - You can send the content of the file as binary. This is used when a file is being uploaded from the browser.</li><li><code>base64</code> - Base64 encoded string of file content.</li><li><code>url</code> - URL of the file from where to download the content before uploading. For example - <code>https://www.example.com/rest-of-the-image-path.jpg</code>.</li></ul><p><strong>Note:</strong> When passing a URL in the file parameter, please ensure that our servers can access the URL. In case ImageKit is unable to download the file from the specified URL, a <code>400</code> error response is returned. In addition to this, the file download request is aborted if response headers are not received in 8 seconds. This will also result in a <code>400</code> error. </p> |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 
| <p><strong>token</strong><br>required</p>                                                   | <p>This is the client-generated JSON Web Token (JWT). The ImageKit.io server uses it to authenticate and check that the upload request parameters have not been tampered with after the generation of the token. Learn how to create the token below on the page.<br><br><strong>Note:</strong> Sending a JWT that has been used in the past will result in a validation error. Even if your previous request resulted in an error, you should always send a new token.</p><span data-gb-custom-inline data-tag="emoji" data-code="26a0">⚠</span><strong>Warning:</strong> JWT must be generated on the server-side because it is generated using your account's private API key. This field is required for authentication when uploading a file from the client-side.</p>                                                                                                                                                                                                                                                                                                                                                                                                |
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

## JSON Web Token (JWT) generation for client-side file upload

JSON Web Token (JWT) is an open standard [(RFC 7519)](https://datatracker.ietf.org/doc/html/rfc7519) that defines a compact and self-contained way for securely transmitting information between parties as a JSON object. Your backend should ideally implement an API that should provide `token`. This is sent along with your upload request for authentication as well as validation of the integrity of upload parameters when using the upload API from the client side. Generating it requires your ImageKit.io [private API key](../api-introduction/api-keys.md#private-key), and hence this should be generated on your backend. Learn more about JSON Web Token [here](https://jwt.io/introduction).

JSON Web Token consist of three parts separated by dots-
- Header
- Payload
- Signature

<b>Header</b>

The header of the JSON Web Tokens consists of three parts: the `typ` of the token, which should be `JWT`, the signing algorithm, `alg`, being used, which is HMAC SHA256, and the `kid`, which is your ImageKit account's public key.

```javascript
{
  "alg": "HS256",
  "typ": "JWT"
  "kid": "your_public_key"
}
```

<b>Payload</b>

The payload is the token's second component. This should include all of the upload parameters that you intend to provide in your upload file request. All arguments except `file` and `token` can be included in this payload. In your upload API request, you must include the same set of parameters and their associated values along with the 'file' and 'token'. If there is a mismatch between upload request parameter and their values and the ones in the payload used to generate JWT, the upload request will fail.

The key in the payload should be the parameter name, and the value should be in stringified form. If you want to send the 'fileName' and 'useUniqueFileName' parameters, for example, the payload will be:

```javascript
{
  "fileName": "dress.jpg",
  "useUniqueFileName": "false" 
}
```

<b>Signature</b>

To create the signature you have to take the encoded header, the encoded payload, your ImageKit.io account's private key and sign that using the HMAC SHA256 algorithm.

```
HMACSHA256(
  base64UrlEncode(header) + "." +
  base64UrlEncode(payload),
  YOUR_PRIVATE_KEY)
```

This is used to ensure that the payload was not altered along the route, as well as that the sender of the JWT is who they claim to be.

To create the JWT, the three outputs are transformed to Base64url strings and concatenated with periods (.). This JWT can then be used as a `token` in the upload API request.

{% hint style="danger" %}
**Never publish your private key on client-side**
The Private API key should be kept confidential and only stored on your own servers.
{% endhint %}

If you are using ImageKit.io [JavaScript SDK](https://github.com/imagekit-developer/imagekit-javascript) for file upload, it requires an `authenticationEndpoint` endpoint for getting authentication parameters required in the upload API.

### How to implement authenticationEndpoint endpoint?

This endpoint is specified by `authenticationEndpoint` parameter during initialization. The SDK sends an HTTP POST request to this endpoint and expects a JSON response having status code 200 containing the JWT in the `token` field.

The request body structure looks like this:

```javascript
{
    // The uploadPayload is the payload that you have to sign using your private key. It must contain all the parameters except 'file' and 'token' that you intend to send in the upload API request.
    "uploadPayload": {
        "fileName": "dress.jpg"
        "useUniqueFileName": "false",
        "tags": "summer,dress",
    },
    "expire": 400, // In seconds, must be less than one hour (3600 seconds)
    "publicKey": "your_public_key",
}
```

Response structure:

```javascript
{
    token: "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c",
}
```

Since calculating these parameters requires your ImageKit.io [private API key](../api-introduction/api-keys.md#private-key), hence this endpoint has to be implemented on your server-side. You can implement this endpoint in any language of your choice. Below is an example of how to implement this endpoint in Node.js using [jsonwebtoken](https://www.npmjs.com/package/jsonwebtoken).

{% tabs %}
{% tab title="Node.js" %}
```javascript
const jwt = require('jsonwebtoken');

const payload = req.body;
const token = jwt.sign(payload.uploadPayload, "your_private_key", {
    expiresIn: payload.expire,
    header: {
      alg: "HS256",
      typ: "JWT",
      kid: payload.publicKey,
    },
});
res.set({
    "Access-Control-Allow-Origin" : "*"
})
res.status(200);
res.send({ token });
```
{% endtab %}
{% endtabs %}

## Examples

The example below demonstrates only basic usage. Refer to [these examples](server-side-file-upload.md#examples) in the server-side upload section to learn about different use-cases. The only difference between client-side and server-side upload is how API authentication works.

Make sure you have implemented `authenticationEndpoint` endpoint on your server as shown [here](secure-client-side-file-upload.md#how-to-implement-authenticationendpoint-endpoint) before using the below examples.

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
        authenticationEndpoint : "https://www.yourserver.com/auth",
        apiVersion: "v2"
    });
    
    // Upload function internally uses the ImageKit.io javascript SDK
    function upload(data) {
        var file = document.getElementById("file1");
        imagekit.upload({
            file : file.files[0],
            fileName : "abc.jpg",
            tags : ["tag1"]
        }, function(err, result) {
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
	
		// Let's get the token from server side
		$.ajax({
		    url : authenticationEndpoint,
		    method : "POST",
		    contentType: "application/json",
            data: JSON.stringify({
                uploadPayload: {
                    fileName: "abc.jpg"
                },
                expire: 3600,
                publicKey: "your_public_api_key"
            }),
		    success : function(body) {
		        formData.append("token", body.token);
	
				// Now call ImageKit.io upload API v2
		        $.ajax({
		            url : "https://upload.imagekit.io/api/v2/files/upload",
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
{% endtabs %}
