# API Introduction

ImageKit.io offers REST API so that you can programmatically integrate ImageKit.io in your application. Currently, we offer APIs for:

1. [Uploading files](../upload-file-api/) in the Media library.
2. [Updating files details](../media-api/update-file-details.md), e.g., tags and custom coordinates for cropping.
3. [Listing and searching files](../media-api/list-and-search-files.md) in the Media library by file/folder name, tags.
4. [Getting individual file details](../media-api/get-file-details.md).
5. [Deleting files](../media-api/delete-file.md).
6. [Delete files in bulk](../media-api/delete-files-bulk.md).
7. [Purging cache](../media-api/purge-cache.md) on CDN.
8. [Getting image metadata](../metadata-api/get-image-metadata-for-uploaded-media-files.md) for an image file.
9. Bulk [add](../media-api/add-tags-bulk.md) or [remove](../media-api/remove-tags-bulk.md) tags.
10. [Copy](../media-api/copy-file.md) or [move](../media-api/move-file.md) files.
11. [Copy](../media-api/copy-folder.md) or [move](../media-api/move-folder.md) folder.
12. [Get bulk job status](../media-api/copy-move-folder-status.md) for copy and move folder jobs.

## Request and response encoding

Except for [upload API](../upload-file-api/), all our APIs accept JSON-encoded request bodies and returns JSON-encoded response.

## Run in Postman

We’ve created a Postman collection to make testing and working with our API simpler.

[![Run in Postman](https://run.pstmn.io/button.svg)](https://app.getpostman.com/run-collection/384637cdb2d49095b113)

## Error codes

ImageKit.io API uses standard HTTP error codes.

| Error code                                                                  | Description                                                                                                                                                                                                                                                                                                                                                                                                        |
| --------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| <p><strong>2xx</strong><br>OK</p>                                           | Everything worked as expected.                                                                                                                                                                                                                                                                                                                                                                                     |
| <p><strong>400</strong><br>Bad request</p>                                  | The request was unacceptable, often due to missing or invalid parameter(s). In this case, a JSON-encoded error response is returned with the `message` property. `message` contains the details about the error and possible solution.                                                                                                                                                                             |
| <p><strong>401</strong><br>Unauthorized<br></p>                             | No valid API key was provided.                                                                                                                                                                                                                                                                                                                                                                                     |
| <p><strong>403</strong></p><p>Forbidden</p>                                 | <p>Can be for the following reasons which will be indicated in the <code>message</code>field in the response:<br></p><ul><li>ImageKit could not authenticate your account with the keys provided.</li><li>An expired key (public or private) was used with the request.</li><li>The account is disabled.</li><li>If you are using the upload API, the total storage limit (or upload limit) is exceeded.</li></ul> |
| <p><strong>429</strong><br>Too Many Requests<br></p>                        | <p>Too many requests hit the API too quickly.<br>We recommend you throttle the request rate as per the <br>value of <code>X-RateLimit-Limit</code> and <code>X-RateLimit-Reset</code> response headers and stay within <a href="rate-limits.md">rate limits</a>.</p>                                                                                                                                               |
| <p><strong>500, 502, 503, 504</strong><br><strong></strong>Server error</p> | <p>Something went wrong with ImageKit.io API. <br>Please create a support ticket by emailing us at <a href="mailto:supprort@imagekit.io">support@imagekit.io</a>.</p>                                                                                                                                                                                                                                              |

## Request ID (x-ik-requestId)

All API response contains a `x-ik-requestId` header. The value of this header is a unique identifier associated with the API request. If you face any issues with any API, then provide this header value in your support ticket to help us troubleshoot the issue quickly.
