# Testing on multiple environments

Now that you have successfully [configured the origin](../integration/configure-origin/) and [URL-endpoint](../integration/url-endpoints.md), its time to test ImageKit.io. It is recommended that you test ImageKit.io on your staging or UAT environment before making changes in the live production application.

You should create separate [URL-endpoint](../integration/url-endpoints.md#how-to-add-a-new-url-endpoint) for dev, stating/UAT, and production environment. This is critical for maintaining separate caches on CDN and ImageKit.io.

## Example setup

Suppose, you are running a website `https://www.example.com`, and you have three different environments:

1. **Dev** - accessible over `http://dev.example.com` \(via host file entry\) or `http://localhost:8000/`
2. **Staging** - accessible over `https://staging.example.com`
3. **Production live website** - `https://www.example.com`

We will add three origins in ImageKit.io dashboard for these three environments. And configure three separate endpoints to [access these origins](../integration/url-endpoints.md#image-origin-preference).

### 💻 Testing on the local machine

Please note that your origin must be accessible over the internet for ImageKit.io to fetch images. You can use a tool like [ngrok](https://ngrok.com/) to make you dev site accessible over the internet.

Start ngrok in a command prompt with the same port number that you have configured for your server \(e.g., `./ngrok http 8000`\). You should see information about your tunnel session such as status, expiration, and version. Take note of the **Forwarding** addresses \(e.g., `https://xxxxxxxx.ngrok.io -> localhost:8000`\) as this is required while adding the origin for dev.

