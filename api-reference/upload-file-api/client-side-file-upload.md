# Client side file upload

You can directly upload files to the ImageKit.io media library from client-side environments such as JavaScript web applications and Android or iOS apps. The API is the same as [server-side upload API](./server-side-file-upload.md), with a minor change in how the authentication works.

Instead of using [private API key](../api-introduction/api-keys.md#private-key), your client-side application must pass authentication parameters. The upload API expects a `signature,` one-time `token`, and `expire` parameters.

Learn how to [implement client-side file upload](#how-to-implement-client-side-file-upload).

{% hint style="info" %}
**File size limit**
The maximum upload file size is limited to 25MB on the free plan. On the paid plan, this limit is 300MB for video files.

**Version limit**
A file can have a maximum of 100 versions.
{% endhint %}

## Endpoint

Same as [server-side file upload API](./server-side-file-upload.md#endpoint)

## Request structure (multipart/form-data)

The request structure is the same as [server-side file upload API](./server-side-file-upload.md#request-structure-multipartform-data), except that the client-side application needs to pass `signature`, one-time `token`, and `expire` parameter. These three parameters are explained below.

| Parameter                                                                                   | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| ------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p><strong>signature</strong><br>required<br></p>                                           | <p>HMAC-SHA1 digest of the <code>token+expire</code> using your ImageKit.io <a href="../api-introduction/api-keys.md#private-key">private API key</a> as a key. Learn how to create a signature on the page below. This should be in lowercase.<br></p><p><span data-gb-custom-inline data-tag="emoji" data-code="26a0">⚠</span> <strong>Warning:</strong> Signature must be calculated on the server-side. This field is required for authentication when uploading a file from the client side.</p>                                                                                                                                                                                                                                                                                                                                                                             |
| <p><strong>expire</strong><br>required</p>                                                  | The time until your signature is valid. It must be a [Unix time](https://en.wikipedia.org/wiki/Unix\_time) in less than 1 hour into the future. It should be in seconds.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| <p><strong>token</strong><br>required</p>                                                   | <p>A unique value that the ImageKit.io server will use to recognize and prevent subsequent retries for the same request. We suggest using V4 UUIDs, or another random string with enough entropy to avoid collisions.<br><br><strong>Note:</strong> Sending a value that has been used in the past will result in a validation error. Even if your previous request resulted in an error, you should always send a new value for this field.</p>                                                                                                                                                                                                                                                                                                                                                                                                 |

## Response code and structure (JSON)

Same as [server-side file upload API](./server-side-file-upload.md#response-code-and-structure-json).

### Understanding response

Refer to this table in [server-side file upload API](./server-side-file-upload.md#understanding-response).


## How to implement client-side file upload?

Here are the steps:

1. The client-side application initiates a request to the backend to obtain authentication parameters. This request should be made to a secure API endpoint accessible only to authenticated users, safeguarding your ImageKit Media library from unauthorized access.
2. The required parameters are generated on the backend using the [private API key](../api-introduction/api-keys.md#private-key). This is explained below with examples in different programming languages.
3. The client-side application then includes these security parameters in the payload of the upload API request.

{% hint style="danger" %}
**Never publish your private key on client-side**
The Private API key should be kept confidential and only stored on your servers.
{% endhint %}

Using ImageKit client-side and server-side SDKs, you can quickly implement upload functionality.
* On the backend, you can use utility functions provided in all [server-side SDKs](../api-introduction/sdk.md#server-side-sdks) to implement the secure API.
* On client-side application, use ImageKit [client-side SDKs](../api-introduction/sdk.md#client-side-sdks) to get started quickly. See [examples](#client-side-file-upload-examples).

### Backend signature generation

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

## Client-side file upload examples

The example below demonstrates only basic usage. Refer to [these examples](server-side-file-upload.md#examples) in the server-side upload section to learn about different use cases. The only difference between client-side and server-side uploads is how API authentication works.

Make sure you have implemented the secure API in the backend that can return the `signature`, one-time `token`, and `expire` parameters.

The best way is to follow [quick start guides](../../getting-started/quickstart-guides/README.md) for your programming language.

{% tabs %}
{% tab title="JavaScipt SDK" %}
{% code title="index.html" %}
```markup
<form action="#" onsubmit="upload()">
  <input type = "file" id="file1" />
  <input type = "submit" />
</form>
<script type="text/javascript" src="../dist/imagekit.js"></script>

<script>
  /*  
    SDK initialization
  */
  const imagekit = new ImageKit({
    publicKey: "your_public_api_key",
    urlEndpoint: "https://ik.imagekit.io/your_imagekit_id",
  });

  // Upload function internally uses the ImageKit.io javascript SDK
  async function upload(data) {
    const file = document.getElementById("file1");
    const authenticationEndpoint = "https://www.yourserver.com/auth";
    const authResponse = await fetch(authenticationEndpoint, {
      method: "GET",
      headers: {
        "Content-Type": "application/json",
      },
    });

    if (!authResponse.ok) {
      throw new Error("Failed to fetch auth details");
    }

    const authData = await authResponse.json();

    imagekit.upload({
      file: file.files[0],
      fileName: "abc.jpg",
      tags: ["tag1"],
      token: authData.token,
      signature: authData.signature,
      expire: authData.expire,
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
  const authenticationEndpoint = "https://www.yourserver.com/auth";
	
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
import { IKContext, IKUpload } from 'imagekitio-react'

function App() {
  const publicKey = "your_public_api_key";
  const urlEndpoint = "https://ik.imagekit.io/your_imagekit_id";
  const authenticator = async () => {
    try {
      // You can also pass headers and validate the request source in the backend, or you can use headers for any other use case.
      const headers = {
        'Authorization': 'Bearer your-access-token',
        'CustomHeader': 'CustomValue'
      };
      const response = await fetch('server_endpoint', {
          headers
      });
      if (!response.ok) {
          const errorText = await response.text();
          throw new Error(`Request failed with status ${response.status}: ${errorText}`);
      }
      const data = await response.json();
      const { signature, expire, token } = data;
      return { signature, expire, token };
    } catch (error) {
        throw new Error(`Authentication request failed: ${error.message}`);
    }
  };

  return (
    <div className="App">
      <p>To use this funtionality please remember to setup the server</p>
      <IKContext publicKey={publicKey} urlEndpoint={urlEndpoint} authenticator={authenticator} >
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
      :authenticator="authenticator"
    >
      <IKUpload fileName="abc.jpg" v-bind:tags="['tag1']" v-bind:responseFields="['tags']"/>
    </IKContext>
    <p>To use this funtionality please remember to setup the server</p>
  </div>
</template>

<script>
import { IKContext, IKUpload } from "imagekitio-vue";

export default {
  name: "app",
  components: {
    IKContext,
    IKUpload
  },
  data() {
    return {
      urlEndpoint: "https://ik.imagekit.io/your_imagekit_id",
      publicKey: "your_public_api_key"   
    };
  },
  methods: {
    authenticator() {
      return new Promise((resolve, reject) => {
        const headers = {
          'Authorization': 'Bearer your-access-token',
          'CustomHeader': 'CustomValue'
        };
        var url = process.env.VUE_APP_YOUR_AUTH_ENDPOINT;
        // Make the Fetch API request
        fetch(url, { method: "GET", mode: "cors", headers }) // Enable CORS mode
          .then((response) => {
            if (!response.ok) {
              throw new Error(`HTTP error! Status: ${response.status}`);
            }
            return response.json();
          })
          .then((body) => {
            var obj = {
              signature: body.signature,
              expire: body.expire,
              token: body.token,
            };
            resolve(obj);
          })
          .catch((error) => {
            reject([error]);
          });
      });
    },
  },
};
</script>
```
{% endtab %}
{% endtabs %}
