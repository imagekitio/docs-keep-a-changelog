# Secure client side file upload (beta)

You can directly upload files to the ImageKit.io media library from client-side environments such as JavaScript web applications and Android or iOS apps. The API is the same as [server-side upload API V2 (beta)](./server-side-file-upload-v2.md), with a minor change in how the authentication works.

Instead of using [private API key](../api-introduction/api-keys.md#private-key) for authentication, your client-side application must pass a JSON Web Token (JWT) as an authentication parameter. The upload API expects a `token` as a parameter.

Learn how to [implement secure client-side file upload](#how-to-implement-secure-client-side-file-upload).

{% hint style="danger" %}
This API is in beta and subject to change.
{% endhint %}

{% hint style="info" %}
**File size limit**
The maximum upload file size is limited to 25MB on the free plan. On the paid plan, this limit is 300MB for video files.

**Version limit**
A file can have a maximum of 100 versions.
{% endhint %}

## Endpoint

Same as [server-side file upload API V2 (beta)](./server-side-file-upload-v2.md#endpoint)

## Request structure (multipart/form-data)

The request structure is the same as the [server-side file upload API](./server-side-file-upload.md#request-structure-multipartform-data), except for secure client-side upload, the application needs to pass a `token`, as explained below.

| Parameter | Description |
|-----------|-------------|
| <p><strong>token</strong><br>required</p> |<p>This is the client-generated JSON Web Token (JWT). The ImageKit.io server uses it to authenticate and check that the upload request parameters have not been tampered with after the token has been generated. Learn how to create the token on the page below.<br><br><strong>Note:</strong> Sending a JWT that has been used in the past will result in a validation error. Even if your previous request resulted in an error, you should always send a new token.</p><span data-gb-custom-inline data-tag="emoji" data-code="26a0">⚠</span><strong>Warning:</strong> JWT must be generated on the server-side because it is generated using your account's private API key. This field is required for authentication when uploading a file from the client-side.</p>|

## Response code and structure (JSON)

Same as [server-side file upload API](./server-side-file-upload.md#response-code-and-structure-json).

## How to implement secure client-side file upload?

Here are the steps:

1. The client-side application initiates a request to the backend to obtain [JSON Web Token (JWT)](secure-client-side-file-upload.md#json-web-token-jwt-for-client-side-file-upload). This request should be made to a secure API endpoint accessible only to authenticated users, safeguarding your ImageKit Media library from unauthorized access.
2. The `token` is generated on the backend using the [private API key](../api-introduction/api-keys.md#private-key). This is explained below with an example.
3. The client-side application then includes the `token` in the payload of the upload API request.

{% hint style="danger" %}
**Never publish your private key on client-side**
The Private API key should be kept confidential and only stored on your servers.
{% endhint %}

### JSON Web Token (JWT) for secure client-side file upload

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

The `iat` (issued at) and `exp` (expiration) are required fields in the payload. The expiration time `exp` must not exceed the issued time `iat` by more than 3600 seconds.

The key in the payload should be the parameter name, and the value should be in stringified form except for `iat` and `exp`. If you want to send the 'fileName' and 'useUniqueFileName' parameters, for example, the payload will be:

```javascript
{
  "fileName": "dress.jpg",
  "useUniqueFileName": "false",
  "iat": 1714035985, // should be integer
  "exp": 1714039585  // should not exceed iat value by 3600
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

### Backend token generation

The client-side application sends a request to the backend and expects a response containing the JSON Web Token (JWT).

The request body structure can look like this:

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

Calculating this parameter requires your ImageKit.io [private API key](../api-introduction/api-keys.md#private-key), so this endpoint must be implemented on your server. You can implement it in any language of your choice. Below is an example of implementing it in Node.js using [jsonwebtoken](https://www.npmjs.com/package/jsonwebtoken).

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

## Secure client-side file upload example

The example below demonstrates only basic usage. Refer to [these examples](server-side-file-upload.md#examples) in the server-side upload section to learn about different use-cases. The only difference between secure client-side and server-side upload is how API authentication works.

Make sure you have implemented the secure API in the backend that returns the token parameter as shown [here](secure-client-side-file-upload.md#backend-token-generation).

{% tabs %}
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
            dataType: "json",
            // You can also pass headers and validate the request source in the backend, or you can use headers for any other use case.
            headers: {
              'Authorization': 'Bearer your-access-token',
              'CustomHeader': 'CustomValue'
            },
            data: JSON.stringify({
                uploadPayload: {
                    fileName: "abc.jpg"
                },
                expire: 3600,
                publicKey: "your_public_api_key"
            }),
            success : function(body) {
                formData.append("token", body.token);

                // Now call ImageKit.io upload API v2 (beta)
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
