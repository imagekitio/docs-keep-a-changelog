---
description: >-
  Listen to webhooks events for pre & post transformations using ImageKit API.
---

## Webhook Support for Pre & Post Transformations

Use webhook to get updates about pre & post transformations. You can use these events to confirm the success of a pre or post transformation on assets.

{% hint style="info" %}
Before receiving pre & post transformation related events, you need to [configure webhooks](../../api-reference/api-introduction/webhooks.md#how-to-configure-a-webhook) in your ImageKit account.
{% endhint %}

# Pre Transformation Events

- `upload.pre-transform.success`
- `upload.pre-transform.error`

## upload.pre-transform.success

This event is triggered when a pre-transform happens successfully.

Example payload looks like below:

```json
{
  "type": "upload.pre-transform.success",
  "id": "c9a09db9-b5e2-49ed-ac2d-dfac88a6457a",
  "created_at": "2023-10-30T14:44:08.165Z",
  "request": {
    "x_request_id": "1bfe7bb6-afb3-4ccf-81f2-6bef7dae48e9",
    "transformation": "rt-90"
  },
  "data": {
    "fileId": "6540dbea002322e4ebe30de2",
    "name": "image_iEyH1dQme.jpg",
    "size": 2300818,
    "versionInfo": {
      "id": "6540dbea002322e4ebe30de2",
      "name": "Version 1"
    },
    "filePath": "/image_iEyH1dQme.jpg",
    "url": "https://stage-ik.imagekit.io/demo/image_iEyH1dQme.jpg",
    "fileType": "image",
    "height": 4608,
    "width": 3456,
    "thumbnailUrl": "https://stage-ik.imagekit.io/demo/tr:n-ik_ml_thumbnail/image_iEyH1dQme.jpg",
    "AITags": null
  }
}
```

Here is the description of all fields.

| Field                  | Description                                                                                                                        |
| ---------------------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| type                   | Type of event.                                                                                                                     |
| id                     | Unique identifier of the event.                                                                                                    |
| createdAt              | Timestamp of the event in ISO8601 format.                                                                                          |
| request.x_request_id   | A unique request id to identify request.                                                                                           |
| request.transformation | The requested transformation as a string.                                                                                          |
| data                   | Same as the Upload API response. Refer [Upload File API](../../api-reference/upload-file-api/README.md) for the payload structure. |

## upload.pre-transform.error

Triggered if an error occurs during the pre-transformation. If the reason seems like an error on the ImageKit side, then raise a support ticket at support@imagekit.io.

Example payload looks like below:

```json
{
  "type": "upload.pre-transform.error",
  "id": "502d1c52-e750-4277-8b64-52d43c2ec022",
  "created_at": "2023-10-31T13:49:28.046Z",
  "request": {
    "x_request_id": "722809dc-d6b7-4357-9262-74cdce554350",
    "transformation": "so-70"
  },
  "data": {
    "name": "vid_aKyz6pCpi.mp4",
    "path": "/myfolder/vid_aKyz6pCpi.mp4",
    "transformation": {
      "error": {
        "reason": "encoding_failed"
      }
    }
  }
}
```

Here is the description of all fields.

| Field                            | Description                                 |
| -------------------------------- | ------------------------------------------- |
| type                             | Type of event.                              |
| id                               | Unique identifier of the event.             |
| createdAt                        | Timestamp of the event in ISO8601 format.   |
| request.x_request_id             | A unique request id to identify request.    |
| request.transformation           | The requested transformation as a string.   |
| data.name                        | Name of the file.                           |
| data.path                        | Path of the file.                           |
| data.transformation.error.reason | Reason for the post transformation failing. |

# Post Transformation Events

- `upload.post-transform.success`
- `upload.post-transform.error`

## upload.post-transform.success

This event is triggered when a post-transform happens successfully. Note that each post-transformation will have it's separate webhook sent.

Example payload looks like below:

```json
{
  "type": "upload.post-transform.success",
  "id": "08c12452-1efb-4c35-a28d-378c94e5674b",
  "created_at": "2023-10-30T15:04:59.462Z",
  "request": {
    "x_request_id": "0658b87f-bce3-41ae-8075-ebc26f44ff30",
    "transformation": {
      "type": "transformation",
      "value": "rt-90"
    }
  },
  "data": {
    "fileId": "653fc60e002322e4ebd5b5b7",
    "url": "http://stage-ik.imagekit.io/demo/video_wCqv9BewP.mp4?tr=rt-90",
    "name": "video_wCqv9BewP.mp4"
  }
}
```

Here is the description of all fields.

| Field                           | Description                                                                                               |
| ------------------------------- | --------------------------------------------------------------------------------------------------------- |
| type                            | Type of event.                                                                                            |
| id                              | Unique identifier of the event.                                                                           |
| createdAt                       | Timestamp of the event in ISO8601 format.                                                                 |
| request.x_request_id            | A unique request id to identify request.                                                                  |
| request.transformation.type     | Type of the requested post-transformation. Can be `transformation`, `abs`, `gif-to-video` or `thumbnail`. |
| request.transformation.value    | Value for the requested type of transformation.                                                           |
| request.transformation.protocol | Only applicable if `request.transformation.type` is `abs`. Possible values are `hls`, `dash`.             |
| data.fileId                     | Unique ID of the originally saved file.                                                                   |
| data.url                        | URL of the requested post transformation.                                                                 |
| data.name                       | Name of the file.                                                                                         |

## upload.post-transform.error

Triggered if an error occurs during the pre-transformation. If the reason seems like an error on the ImageKit side, then raise a support ticket at support@imagekit.io.

Example payload looks like below:

```json
{
  "type": "upload.post-transform.error",
  "id": "738f13b0-05e9-40cd-a856-8cc3e48b6e94",
  "created_at": "2023-10-31T13:51:10.627Z",
  "request": {
    "x_request_id": "49022ce9-e532-4a6c-8321-52925d519eec",
    "transformation": {
      "type": "transformation",
      "value": "so-70"
    }
  },
  "data": {
    "fileId": "65410644002322e4ebe6c880",
    "url": "http://stage-ik.imagekit.io/demo/vid_93HC1Daow.mp4?tr=so-70",
    "name": "vid_93HC1Daow.mp4",
    "path": "/vid_93HC1Daow.mp4",
    "transformation": {
      "error": {
        "reason": "encoding_failed"
      }
    }
  }
}
```

Here is the description of all fields.

| Field                            | Description                                                                                               |
| -------------------------------- | --------------------------------------------------------------------------------------------------------- |
| type                             | Type of event.                                                                                            |
| id                               | Unique identifier of the event.                                                                           |
| createdAt                        | Timestamp of the event in ISO8601 format.                                                                 |
| request.x_request_id             | A unique request id to identify request.                                                                  |
| request.transformation.type      | Type of the requested post-transformation. Can be `transformation`, `abs`, `gif-to-video` or `thumbnail`. |
| request.transformation.value     | Value for the requested type of transformation.                                                           |
| request.transformation.protocol  | Only applicable if `request.transformation.type` is `abs`. Possible values are `hls`, `dash`.             |
| data.fileId                      | Unique ID of the originally saved file.                                                                   |
| data.url                         | URL of the requested post transformation.                                                                 |
| data.name                        | Name of the file.                                                                                         |
| data.path                        | Path of the file.                                                                                         |
| data.transformation.error.reason | Reason for the post transformation failing.                                                               |
