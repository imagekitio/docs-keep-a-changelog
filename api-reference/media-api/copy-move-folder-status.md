# Bulk job status

{% api-method method="get" host="https://api.imagekit.io" path="/v1/bulkJobs/:jobId" %}
{% api-method-summary %}
Bulk job status API
{% endapi-method-summary %}

{% api-method-description %}
This endpoint allows you to get the status of a bulk operation e.g. copy or move folder API.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="jobId" type="string" required=false %}
The `jobId` you get in the response of bulk job API e.g. copy folder or move folder API.
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="Authorization" type="string" required=true %}
base64 encoding of `your_private_api_key:`  
**Note the colon in the end.**
{% endapi-method-parameter %}
{% endapi-method-headers %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
On success, you will receive a JSON-encoded response like below.
{% endapi-method-response-example-description %}

```javascript
{
  "jobId": "598821f949c0a938d57563bd",
  "type": "COPY_FOLDER",
  "status": "Completed" // or "Pending"
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

### Response structure and status code \(application/JSON\)

In case of error, you will get an [error code](../api-introduction/#error-codes) along with the error message. On success, you will receive a `200` status code with a JSON-encoded body.

### Understanding the response

The JSON-encoded response will have the following properties:

<table>
  <thead>
    <tr>
      <th style="text-align:left">Property name</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">jobId</td>
      <td style="text-align:left">The <code>jobId</code> you get in the response of bulk job API e.g. copy
        folder or move folder API.</td>
    </tr>
    <tr>
      <td style="text-align:left">type</td>
      <td style="text-align:left">The type of operation, it could be either <code>COPY_FOLDER</code> or <code>MOVE_FOLDER</code>.</td>
    </tr>
    <tr>
      <td style="text-align:left">status</td>
      <td style="text-align:left">
        <p>The current status of the job. It can be either:
          <br />
        </p>
        <ul>
          <li><code>Pending</code> - The job has been successfully submitted and is in
            progress.</li>
          <li><code>Completed</code> - The job has been completed.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

