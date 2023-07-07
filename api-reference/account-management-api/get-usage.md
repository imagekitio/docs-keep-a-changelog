# Get account usage API (beta)

{% swagger baseUrl="https://api.imagekit.io" path="/v1/accounts/usage" method="get" summary="Get account usage information (beta)" %}
{% swagger-description %}
Get the account usage information between two dates.
{% endswagger-description %}

{% swagger-parameter in="header" name="Authorization" required="true" %}
base64 encoding of `your_private_api_key:`

**Note the colon in the end.**
{% endswagger-parameter %}

{% swagger-parameter in="query" name="startDate" %}
Specify a `startDate` in `YYYY-MM-DD` format. It should be before the `endDate`. The difference between `startDate` and `endDate` should be less than 90 days.
{% endswagger-parameter %}

{% swagger-parameter in="query" name="endDate" %}
Specify a `endDate` in `YYYY-MM-DD` format. It should be after the `startDate`. The difference between `startDate` and `endDate` should be less than 90 days.
{% endswagger-parameter %}

{% swagger-response status="200" description="In the response, you will get different usage metrics" %}
```javascript
{
    "bandwidth_bytes": 21991583578,
    "media_library_storage_bytes": 1878758298,
    "video_processing_units_count": 0,
    "extension_units_count": 0,
    "original_cache_storage_bytes": 0
}
```
{% endswagger-response %}
{% endswagger %}



## Examples

Get usage between `2023-04-01` and `2023-03-01`.

{% tabs %}
{% tab title="cURL" %}
```basic
curl -X GET "https://api.imagekit.io/v1/accounts/usage?startDate=2023-04-01&endDate=2023-03-01" \
-H 'Content-Type: application/json' \
-u your_private_key:
```
{% endtab %}

{% endtabs %}
