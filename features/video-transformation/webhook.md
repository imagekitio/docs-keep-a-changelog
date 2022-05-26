# Webhook support for video

Webhook can be used to get updates on ongoing video transformations on ImageKit's video API.

## Receiving events

Imagekit will submit HTTP(S) POST to configured webhook URL, with a JSON body.

### Request body

All webhook bodies are JSON encoded. The schema of the body may differ based on the event type, but the following fields are identical in all.

| Field         | DataType | Description                              |
| ------------- | -------- | ---------------------------------------- |
| **type**      | `string` | Type of event                            |
| **id**        | `string` | Unique identifier of the event           |
| **createdAt** | `string` | Timestamp of the event as ISO8601 string |

### Event types

- `video.transformation.accepted` - When a transformation request is accepted.
- `video.transformation.ready` - When a transformation is ready and available in cache.
- `video.transformation.error` - When a transformation request fails with an error.

#### Event: `video.transformation.accepted`

Sent when new video transformation request is accepted for processing.

| Field                                           | DataType    | Description                                                                                |
| ----------------------------------------------- | ----------- | ------------------------------------------------------------------------------------------ |
| **type**                                        | `string`    | Type of event                                                                              |
| **id**                                          | `string`    | Unique identifier of the event                                                             |
| **createdAt**                                   | `string`    | Timestamp of the event as ISO8601 string                                                   |
| **request.url**                                 | `string`    | URL of submitted request                                                                   |
| **request.x_request_id**                        | `string`    | UniqueId of submitted request                                                              |
| **request.user_agent**                          | `string`    | `user_agent` in submitted request                                                          |
| **data.asset.url**                              | `string`    | URL to download source video file                                                          |
| **data.transformation.type**                    | `string`    | `“video-transformation”\| ”gif-to-video”\| ”video-thumbnail”`                              |
| **data.transformation.options.video_codec**     | `string?`   | `“h264”\|”vp9”`                                                                            |
| **data.transformation.options.audio_codec**     | `string?`   | `“aac”\|"opus"`                                                                            |
| **data.transformation.options.format**          | `string`    | `“mp4”\|”webm”\|”jpg”\|”png”\|”webp”`                                                      |
| **data.transformation.options.stream_protocol** | `string?`   | ABS protocol. `HLS \| "DASH"`                                                              |
| **data.transformation.options.variants**        | `string[]?` | ABS protocol. `Array<“360“\|480”\|”720”\|”1028”>`                                          |
| **data.transformation.options.auto_rotate**     | `boolean`   | Autorotate input images & videos based on their metadata, if no `rt` param applied to them |

Example

```js
{
  type: "video.transformation.accepted",
  id: "f30b0f2f-145e-4591-8e56-a6d0168fe123",
  created_at: "2022-05-25T08:53:28.143Z",
  request: {
    x_request_id: "123456712",
    url: "https://www.learningcontainer.com/wp-content/uploads/2020/05/sample-mp4-file.mp4",
    user_agent: "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/80.0.3987.122 Safari/537.36",
  },
  data: {
    asset: {
      url: "https://www.learningcontainer.com/wp-content/uploads/2020/05/sample-mp4-file.mp4",
    },
    transformation: {
      type: "video-transformation",
      options: {
        video_codec: "vp9",
        audio_codec: "opus",
        auto_rotate: true,
        quality: 10,
        format: "webm",
      },
    },
  },
}
```

#### Event: `video.transformation.ready`

Sent when a transformation is ready and available in cache.

| Field                                                  | DataType    | Description                                                                                |
| ------------------------------------------------------ | ----------- | ------------------------------------------------------------------------------------------ |
| **type**                                               | `string`    | Type of event                                                                              |
| **id**                                                 | `string`    | Unique identifier of the event                                                             |
| **createdAt**                                          | `string`    | Timestamp of the event as ISO8601 string                                                   |
| **request.url**                                        | `string`    | URL of submitted request                                                                   |
| **request.x_request_id**                               | `string`    | UniqueId of submitted request                                                              |
| **request.user_agent**                                 | `string`    | `user_agent` in submitted request                                                          |
| **data.asset.url**                                     | `string`    | URL to download source video file                                                          |
| **data.transformation.type**                           | `string`    | `“video-transformation”\| ”gif-to-video”\| ”video-thumbnail”`                              |
| **data.transformation.options.video_codec**            | `string?`   | `“h264”\|”vp9”`                                                                            |
| **data.transformation.options.audio_codec**            | `string?`   | `“aac”\|"opus"`                                                                            |
| **data.transformation.options.format**                 | `string`    | `“mp4”\|”webm”\|”jpg”\|”png”\|”webp”`                                                      |
| **data.transformation.options.stream_protocol**        | `string?`   | ABS protocol. `"HLS"\| "DASH"`                                                             |
| **data.transformation.options.variants**               | `string[]?` | ABS protocol. `Array<“360“\|480”\|”720”\|”1028”>`                                          |
| **data.transformation.options.auto_rotate**            | `boolean`   | Autorotate input images & videos based on their metadata, if no `rt` param applied on them |
| **data.transformation.output.video_metadata.duration** | `number`    | Output duration in seconds (Float number)                                                  |
| **data.transformation.output.video_metadata.bitrate**  | `number`    | Output bitrate (bits/second)                                                               |
| **data.transformation.output.video_metadata.width**    | `number`    | Output width (no rotation meta data expected)                                              |
| **data.transformation.output.video_metadata.height**   | `number`    | Output height (no rotation meta data expected)                                             |
| **data.transformation.output.url**                     | `string`    | URL to download output file                                                                |
| **data.timings.download_duration**                     | `string`    | Duration of download in milliseconds                                                       |
| **data.timings.encoding_duration**                     | `string`    | Encoding of download in milliseconds                                                       |

Example

```js
{
  type: "video.transformation.ready",
  id: "f30b0f2f-145e-4591-8e56-a6d0168fe123",
  created_at: "2022-05-25T08:53:28.143Z",
  request: {
    x_request_id: "123456712",
    url: "https://www.learningcontainer.com/wp-content/uploads/2020/05/sample-mp4-file.mp4",
    user_agent: "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/80.0.3987.122 Safari/537.36",
  },
  timings: {
    donwload_duration: 3351,
    encoding_duration: 2353,
  },
  data: {
    asset: {
      url: "https://www.learningcontainer.com/wp-content/uploads/2020/05/sample-mp4-file.mp4",
    },
    transformation: {
      type: "video-transformation",
      options: {
        video_codec: "vp9",
        audio_codec: "opus",
        auto_rotate: true,
        quality: 10,
        format: "webm",
      },
      output: {
        url: "https://www.learningcontainer.com/wp-content/uploads/2020/05/sample-mp4-file.mp4",
        video_metadata: {
          duration: 2.037,
          width: 854,
          height: 480,
          bitrate: 2.037,
        },
      },
    },
  },
}
```

#### Event: `video.transformation.error`

Sent when a transformation request fails with an error.

| Field                                           | DataType    | Description                                                                                |
| ----------------------------------------------- | ----------- | ------------------------------------------------------------------------------------------ |
| **type**                                        | `string`    | Type of event                                                                              |
| **id**                                          | `string`    | Unique identifier of the event                                                             |
| **createdAt**                                   | `string`    | Timestamp of the event as ISO8601 string                                                   |
| **request.url**                                 | `string`    | URL of submitted request                                                                   |
| **request.x_request_id**                        | `string`    | UniqueId of submitted request                                                              |
| **request.user_agent**                          | `string`    | `user_agent` in submitted request                                                          |
| **data.asset.url**                              | `string`    | URL to download source video file                                                          |
| **data.transformation.type**                    | `string`    | `“video-transformation”\| ”gif-to-video”\| ”video-thumbnail”`                              |
| **data.transformation.options.video_codec**     | `string?`   | `“h264”\|”vp9”`                                                                            |
| **data.transformation.options.audio_codec**     | `string?`   | `“aac”\|"opus"`                                                                            |
| **data.transformation.options.format**          | `string`    | `“mp4”\|”webm”\|”jpg”\|”png”\|”webp”`                                                      |
| **data.transformation.options.stream_protocol** | `string?`   | ABS protocol. `HLS\|"DASH"`                                                                |
| **data.transformation.options.variants**        | `string[]?` | ABS protocol. `Array<“360“\|480”\|”720”\|”1028”>`                                          |
| **data.transformation.options.auto_rotate**     | `boolean`   | Autorotate input images & videos based on their metadata, if no `rt` param applied on them |
| **data.transformation.error.reason**            | `string`    | `"ENCODING_FAILED"\|"DOWNLOAD_FAILED"\|"INTERNAL_SERVER_ERROR`                             |

Example

```js
{
  type: "video.transformation.ready",
  id: "f30b0f2f-145e-4591-8e56-a6d0168fe123",
  created_at: "2022-05-25T08:53:28.143Z",
  request: {
    x_request_id: "123456712",
    url: "https://www.learningcontainer.com/wp-content/uploads/2020/05/sample-mp4-file.mp4",
    user_agent: "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/80.0.3987.122 Safari/537.36",
  },
  data: {
    asset: {
      url: "https://www.learningcontainer.com/wp-content/uploads/2020/05/sample-mp4-file.mp4",
    },
    transformation: {
      type: "video-transformation",
      options: {
        video_codec: "vp9",
        audio_codec: "opus",
        auto_rotate: true,
        quality: 10,
        format: "webm",
      },
    },
  },
}
```