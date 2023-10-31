# API Introduction

ImageKit.io offers REST API so that you can programmatically integrate ImageKit.io into your application. Currently, we offer APIs for:

* [File upload](../upload-file-api/) - Upload files from server or client-side.
* [Media management](../media-api/) - Integrate easy-to-use media management APIs in your application for searching, updating, copying, moving, renaming and deleting media.
* [File metadata](../metadata-api/) - Get embedded file metadata.
* [Custom metadata fields management](../custom-metadata-fields-api) - Imagekit.io allows you to define a schema for your custom metadata keys. Value filled against that key will have to adhere to those rules. You can create, read and update custom metadata rules and update your file with custom metadata value in file update API or file upload API.


## Request and response encoding

Except for [upload API](../upload-file-api/), all our APIs accept JSON-encoded request bodies and returns JSON-encoded response.

## Run in Postman

We’ve created a Postman collection to make testing and working with our API simpler.

[![Run in Postman](https://run.pstmn.io/button.svg)](https://app.getpostman.com/run-collection/e8afe4815e95f8b14660)

## OpenAPI specification
Here is the OpenAPI specification for [v1 version](https://github.com/imagekitio/openapi/blob/master/v1.0.0.yaml) of ImageKit's API. See a [working demo](https://elements-demo.stoplight.io/?spec=https://raw.githubusercontent.com/imagekitio/openapi/master/v1.0.0.yaml) here in Stoplight.

## Error codes

ImageKit.io API uses standard HTTP error codes.

| Error code                                                                  | Description                                                                                                                                                                                                                                                                                                                                                                                                        |
| --------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| <p><strong>2xx</strong><br>OK</p>                                           | Everything worked as expected.                                                                                                                                                                                                                                                                                                                                                                                     |
| <p><strong>400</strong><br>Bad request</p>                                  | The request was unacceptable, often due to missing or invalid parameter(s). In this case, a JSON-encoded error response is returned with the `message` property. `message` contains the details about the error and possible solution.                                                                                                                                                                             |
| <p><strong>401</strong><br>Unauthorized<br></p>                             | No valid API key was provided.                                                                                                                                                                                                                                                                                                                                                                                     |
| <p><strong>403</strong></p><p>Forbidden</p>                                 | <p>Can be for the following reasons which will be indicated in the <code>message</code>field in the response:<br></p><ul><li>ImageKit could not authenticate your account with the keys provided.</li><li>An expired key (public or private) was used with the request.</li><li>The account is disabled.</li><li>If you are using the upload API, the total storage limit (or upload limit) is exceeded.</li></ul> |
| <p><strong>429</strong><br>Too Many Requests<br></p>                        | <p>Too many requests hit the API too quickly.<br>We recommend you throttle the request rate as per the <br>value of <code>X-RateLimit-Limit</code> and <code>X-RateLimit-Reset</code> response headers and stay within <a href="rate-limits.md">rate limits</a>.</p>                                                                                                                                               |
| <p><strong>500, 502, 503, 504</strong><br><strong></strong>Server error</p> | <p>Something went wrong with ImageKit.io API. <br>Please create a support ticket by emailing us at <a href="mailto:supprort@imagekit.io">support@imagekit.io</a>.</p>                                                                                                                                                                                                                                              |

## Request ID (x-ik-requestId)

All API response contains a `x-ik-requestId` header. The value of this header is a unique identifier associated with the API request. If you face any issues with any API, then provide this header value in your support ticket to help us troubleshoot the issue quickly.
