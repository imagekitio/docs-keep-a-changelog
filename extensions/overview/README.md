---
description: >-
  Extended capabilities for advanced processing on your assets such as
  background removal, auto-tagging, etc.
---

# Extensions reference

### Available extensions

* [Automatic tagging of images](ai-based-auto-tagging.md)
* [Background removal from images](background-removal.md)

### What are extensions?

Extensions let you perform certain advanced operations on your assets. ImageKit applies them to your assets in an integrated manner. To do this, ImageKit leverages functionality provided by third-party services using APIs. For example, when you apply the background removal extension, ImageKit consumes the [remove.bg](https://www.remove.bg) API to perform the actual background removal operation, and then uploads the resulting image to your media library. You only need to specify the name of the extension to be performed on any particular asset, and possibly a few extension-specific parameters.

Extensions are optional and must be explicitly specified in your API call. Extensions may execute asynchronously after the upload or update operation has been completed and you receive your API response. In these cases, a webhook URL can be specified. Once the extensions have been executed completely, ImageKit will send a POST HTTP request to this URL with the status of success or failure of the operation, and the result or error respectively. For complete API reference, refer to the [upload](https://docs.imagekit.io/api-reference/upload-file-api) and [update](https://docs.imagekit.io/api-reference/media-api/update-file-details) API reference pages.

### How to apply extensions on assets?

1. **While uploading a new asset:**\
   During an [upload](../../media-library/overview/upload-files.md) API call, you can specify the extensions that you want ImageKit to apply to the asset. The extensions will be applied only after the asset has been successfully uploaded.\

2. **While updating an already existing asset:**\
   You can apply extensions on an existing asset through an [update](../../api-reference/media-api/update-file-details.md) API call. The extensions will be applied only after the asset has been successfully updated.

The `extensions` parameter of both upload and update APIs takes an array of objects specifying the extensions to be used along with their respective parameters. Each object in this array is a self-contained block containing information about one single extension. This information includes the `name` of the extension and other parameters associated with that extension.

| Field Name | Type   | Required | Description                                                                                                                                                      |
| ---------- | ------ | -------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **name**   | string | Yes      | <p>Each extension has a unique name associated with it. <br>Example: <code>google-auto-tagging</code>, <code>aws-auto-tagging</code>, <code>remove-bg</code></p> |

Apart from the `name`, there are a few fields that are supported or required only when using certain extensions. Refer to [auto tagging](ai-based-auto-tagging.md#extensions-parameter-for-auto-tagging) and [background removal](background-removal.md#extensions-parameter-for-background-removal) pages for information on their specific parameters.

Here's an example of an `extensions` parameter formation that will result in the execution of two different extensions - background removal through [remove.bg](https://www.remove.bg) and automatic tagging through [Google Cloud Vision](https://cloud.google.com/vision/docs/labels):

```javascript
// Request body for an update/upload API call
{
    /*
    ...rest of the update/upload request parameters
    */
    "extensions": [
        {
            "name": "remove-bg",
            "options": { // all parameters inside this object are sent directly to the third-party service
                "add_shadow": true
            }
        },
        {
            "name": "google-auto-tagging",
            "minConfidence": 80, // only tags with a confidence value higher than 80% will be attached
            "maxTags": 10 // a maximum of 10 tags will be attached
        }
    ]
}
```

### Extension pricing

Your ImageKit plan includes a fixed amount of extension units that can be used to execute different extensions. Different extension consumes a different amount of fixed units.

| Extension           | Unit consumed |
| ------------------- | ------------- |
| aws-auto-tagging    | 1             |
| google-auto-tagging | 2             |
| remove-bg           | 130           |

So for example, if you removed background from 10 images, a total of 130x10 i.e. 1300 extension units will be consumed.

If you add tags using `aws-auto-tagging` extension on 1000 uploaded images, a total of 1000x2 i.e. 2000 extension units will be consumed.

### Asynchronous behavior

Extensions may execute asynchronously after your main update/upload call has successfully completed and you have received the response for it. Certain extensions take a longer time than others to execute; for example, [background removal](background-removal.md). These are **always **performed asynchronously. Certain extensions however are first tried to be performed synchronously, e.g. [auto tagging](ai-based-auto-tagging.md) (both Google Cloud Vision and AWS Rekognition). 

If performed synchronously, the results of these extensions will be integrated with the main response of the update/upload API call. However, there is no guarantee that any extension will be performed synchronously. You can inspect the response field `extensionStatus` to identify the method used for each extension in your request.

### Response structure

If an update/upload API call includes a valid `extensions` field array with a non-zero length, then the response will include an `extensionStatus` field object, which will contain the status of each extension at the time of completion of the update/upload request. The status has three possible values:

* `success`: The extension has been successfully and fully applied
* `failed`: The extension has failed and will not be retried
* `pending`: The extension will finish processing in some time. On completion, the final status (success / failed) will be sent to the webhook URL provided

The response for an API call with the above request body would look like this

```javascript
// Response body for an update/upload API call
{
    /*
    ...rest of the update/upload response fields
    */
    "AITags": [
        {
            "name": "Shirt",
            "confidence": 90.12,
            "source": "google-auto-tagging"
        },
        /* ... more tags ... */
    ],
    "extensionStatus": {
        "google-auto-tagging": "success",
        "remove-bg": "pending"
    }
}
```

The `AITags` field is populated only because the `google-auto-tagging` extension was executed synchronously and it received a `success`response.

### Webhooks

You can include a `webhookUrl` parameter in your update/upload API calls. The final status of extensions after they have completed execution will be delivered to this endpoint as a POST request. The request body sent to this endpoint will look like this

#### Delivery attempts and retries

ImageKit expects a response with a status code 200 from your webhook endpoint. Any other status code will be interpreted as a failure and may trigger a retry. Not receiving a response in 30 seconds will also be interpreted as a failure. ImageKit will attempt webhook retries for a maximum of 24 hours in spaced intervals.

```javascript
// A success webhook request for a background removal extension
{
    "service": "remove-bg",
    "status": "SUCCESS",
    "fileObject": {
        "fileId": "5effaa5662679b5af2c58829",
        /*
        ... rest of the fields in a file object
        */
    },
}
```

```javascript
// A failure webhook request for a google auto tagging extension
{
    "service": "google-auto-tagging",
    "status": "FAILURE",
    "fileId": "5effaa5662679b5af2c58829",
    "error": {
        /* error fields */
    }
}
```

| Request Field                                                         | Description                                                                                                                                              |
| --------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **service**                                                           | Name of the extension                                                                                                                                    |
| **status**                                                            | Final status of the extension execution. Can be either `SUCCESS` or `FAILURE`                                                                            |
| <p><strong>fileObject</strong></p><p>(for success responses only)</p> | The result of a [Get File Details](../../api-reference/media-api/get-file-details.md) API call that you would receive after completion of this extension |
| <p><strong>fileId</strong></p><p>(for failure responses only)</p>     | Unique id associated with the file on which the extension was attempted                                                                                  |
| <p><strong>error</strong></p><p>(for failure responses only)</p>      | Error object indicating the cause of failure                                                                                                             |
