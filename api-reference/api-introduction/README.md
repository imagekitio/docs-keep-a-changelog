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

| Error code | Description |
| :--- | :--- |
| **2xx** OK | Everything worked as expected. |
| **400** Bad request | The request was unacceptable, often due to missing or invalid parameter\(s\). In this case a JSON-encoded error response is returned with the following properties: |
| **401** Unauthorized  | No valid API key provided. |
| **429** Too Many Requests  | Too many requests hit the API too quickly. We recommend you to throttle request rate as per the  value of `X-RateLimit-Limit` and `X-RateLimit-Reset` response headers and stay within [rate limits](rate-limits.md). |
| **500, 502, 503, 504** Server error | Something went wrong with ImageKit.io API.  Please create a support ticket by emailing us at [support@imagekit.io](mailto:supprort@imagekit.io). |

## Request ID \(x-ik-requestId\)

All API response contains a `x-ik-requestId` header. The value of this header is a unique identifier associated with the API request. If you face any issues with any API, then provide this header value in your support ticket to help us troubleshoot the issue quickly.

