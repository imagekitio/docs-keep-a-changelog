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
  "jobId": "598821f949c0a938d57563bd",
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

{% tab title="Ruby" %}
 ```ruby
imagekitio = ImageKitIo::Client.new("your_private_key", "your_public_key", "your_url_endpoint")
imagekitio.bulk_job_status(job_id: 'job_id')
```
{% endtab %}
{% endtabs %}
