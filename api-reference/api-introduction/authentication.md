# Authentication

Your API requests are authenticated using the account's [private API key](api-keys.md#private-key). If you do not include your key when making an API request or use an incorrect one, we return an error. You can view your API keys in the ImageKit.io dashboard under the [developer's tab](https://imagekit.io/dashboard/developer/api-keys).

Authentication to the API is performed via [HTTP Basic Auth](http://en.wikipedia.org/wiki/Basic_access_authentication). Provide your [private API key](api-keys.md#private-key) as the basic auth username value. You do not need to provide a password.

{% hint style="warning" %}
**Only HTTPS supported**\
All API requests must be made over HTTPS. Calls made over plain HTTP will fail. API requests without authentication will also fail.
{% endhint %}

## Examples

You can provide the private API key in a curl request as:

```bash
curl https://api.imagekit.io/v1/files \ 
-u your_private_api_key:
# The colon prevents curl from asking for a password.
```

You can also use the "Authorization" header and provide base64 encoded value of the string `your_private_api_key:`

{% hint style="info" %}
Notice the colon (:) after the private key. It is required otherwise, authentication will fail. The format is `username:password`. `username` is your private key and `password` is an empty string.
{% endhint %}

If you encode `your_private_api_key:` using base64, you will get `eW91cl9wcml2YXRlX2FwaV9rZXk6`

```bash
curl https://api.imagekit.io/v1/files \
  -H 'Authorization: Basic eW91cl9wcml2YXRlX2FwaV9rZXk6'
```
