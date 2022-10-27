# Bulk job status

{% swagger baseUrl="https://api.imagekit.io" path="/v1/bulkJobs/:jobId" method="get" summary="Bulk job status API" %}
{% swagger-description %}
This endpoint allows you to get the status of a bulk operation e.g. copy or move folder API.
{% endswagger-description %}

{% swagger-parameter in="path" name="jobId" type="string" %}
The `jobId` you get in the response of bulk job API e.g. copy folder or move folder API.
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authorization" type="string" %}
base64 encoding of `your_private_api_key:`

**Note the colon in the end.**
{% endswagger-parameter %}

{% swagger-response status="200" description="On success, you will receive a JSON-encoded response like below." %}
```javascript
{
  "jobId": "job_id",
  "type": "COPY_FOLDER",
  "status": "Completed" // or "Pending"
}
```
{% endswagger-response %}
{% endswagger %}

### Response structure and status code (application/JSON)

In case of an error, you will get an [error code](../api-introduction/#error-codes) along with the error message. On success, you will receive a `200` status code with a JSON-encoded body.

### Understanding the response

The JSON-encoded response will have the following properties:

| Property name | Description                                                                                                                                                                                                                  |
| ------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| jobId         | The `jobId` you get in the response of bulk job API e.g. copy folder or move folder API.                                                                                                                                     |
| type          | The type of operation, it could be either `COPY_FOLDER` or `MOVE_FOLDER`.                                                                                                                                                    |
| status        | <p>The current status of the job. It can be either:<br></p><ul><li><code>Pending</code> - The job has been successfully submitted and is in progress.</li><li><code>Completed</code> - The job has been completed.</li></ul> |

{% tabs %}
{% tab title="cURL" %}
```bash
curl -X GET "https://api.imagekit.io/v1/bulkJobs/job_id" \
-u your_private_api_key:
```
{% endtab %}

{% tab title="Node.js" %}
```javascript
var ImageKit = require("imagekit");

var imagekit = new ImageKit({
    publicKey : "your_public_api_key",
    privateKey : "your_private_api_key",
    urlEndpoint : "https://ik.imagekit.io/your_imagekit_id/"
});

var jobId = "job_id";
imagekit.getBulkJobStatus(jobId, function(error, result) {
    if(error) console.log(error);
    else console.log(result);
});
```
{% endtab %}

{% tab title="Python" %}
```python
from imagekitio import ImageKit

imagekit = ImageKit(
    public_key='your_public_api_key',
    private_key='your_private_api_key',
    url_endpoint = 'https://ik.imagekit.io/your_imagekit_id/'
)

job_status = imagekit.get_bulk_job_status(job_id="job_id")

print("Bulk job status-", job_status, end="\n\n")

# Raw Response
print(job_status.response_metadata.raw)

# print the job's id
print(job_status.job_id)

# print the status
print(job_status.status)
```
{% endtab %}

{% tab title="PHP" %}
```php
use ImageKit\ImageKit;

$public_key = "your_public_api_key";
$your_private_key = "your_private_api_key";
$url_end_point = "https://ik.imagekit.io/your_imagekit_id";

$imageKit = new ImageKit(
    $public_key,
    $your_private_key,
    $url_end_point
);

$jobId = 'job_id';
// The jobId you get in the response of bulk job API e.g. copy folder or move folder API.

$bulkJobStatus = $imageKit->getBulkJobStatus($jobId);

echo("Bulk Job Status : " . json_encode($bulkJobStatus));
```
{% endtab %}

{% tab title="Java" %}
```java
String jobId = "job_id";
ResultBulkJobStatus resultBulkJobStatus = ImageKit.getInstance().getBulkJobStatus(jobId);
```
{% endtab %}

{% tab title="Ruby" %}
 ```ruby
imagekitio = ImageKitIo::Client.new("your_private_key", "your_public_key", "your_url_endpoint")
imagekitio.bulk_job_status(job_id: 'job_id')
```
{% endtab %}

{% tab title="Go" %}
```go
resp, err := ik.BulkJobStatus(ctx, "job_id")
```
{% endtab %}

{% tab title=".Net" %}
```.net
var imagekit = new ImageKit({
    publicKey : "your_public_api_key",
    privateKey : "your_private_api_key",
    urlEndpoint : "https://ik.imagekit.io/your_imagekit_id/"
});
var jobId = "job_id";
ResultBulkJobStatus resultBulkJobStatus = imagekit.GetBulkJobStatus(jobId);
```
{% endtab %}

{% endtabs %}
