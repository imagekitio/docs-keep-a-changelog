# Azure Blob storage

You can configure ImageKit.io to fetch images from your Azure blob storage. This allows you to start leveraging ImageKit.io real-time image resizing, optimization, and fast CDN delivery for thousands or millions of existing images within minutes.

**Note:** We do not start copying images from your storage as soon as you add it. Instead, we will fetch the particular image when you request it through ImageKit.io URL-endpoint. [Learn more](../how-it-works.md) to understand how this works. The images accessed from this origin will not appear in your [Media library](../../media-library/overview/).

## Step 1: Configure origin

1. Go to the [external storage section](https://imagekit.io/dashboard#external-storage) in your ImageKit.io dashboard, and under the Origins section, click on the "Add origin" button.
2. Choose **Web server** from the origin type dropdown.
3. Give your origin a name. It will appear in the list of origins you have added. For example - **My Azure storage**.
4. Enter `https://storagesamples.blob.core.windows.net` in the base URL field.
5. Leave the [advanced options](web-server-origin.md#advanced-options-for-web-server-origin) as it is for now.
6. Click on the Submit button.

{% hint style="info" %}
**Public read access to blob needed**  
The integration with Azure blob storage works like [web server](web-server-origin.md) integration. It requires [public read access for blobs](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-manage-access-to-resources#grant-anonymous-users-permissions-to-containers-and-blobs).
{% endhint %}

## Step 2: Access the image through ImageKit.io URL-endpoint

When you add your first origin in the dashboard, the origin is by default made accessible through the [default URL-endpoint](../url-endpoints.md#default-url-endpoint) of your ImageKit.io account. For subsequent origins, you can either create a separate URL-endpoint or edit existing URL-endpoint \(including default\) and make this newly added origin accessible by editing the [origin preference list](../url-endpoints.md#image-origin-preference). 

Let's look at a few examples to fetch the images:

* **Original image through Azure blog storage \(old URL\)** [https://storagesamples.blob.core.windows.net/rest-of-the-path.jpg](https://storagesamples.blob.core.windows.net/rest-of-the-path.jpg)
* **The same master image using ImageKit.io URL-endpoint** [https://ik.imagekit.io/your\_imagekit\_id/rest-of-the-path.jpg](https://ik.imagekit.io/your_imagekit_id/rest-of-the-path.jpg)
* **Resized 300x300 image** [https://ik.imagekit.io/your\_imagekit\_id/`tr:w-300,h-300`/rest-of-the-path.jpg](https://ik.imagekit.io/your_imagekit_id/tr:w-300,h-300/rest-of-the-path.jpg)

So when you request `https://ik.imagekit.io/your_imagekit_id/rest-of-the-path.jpg`, ImageKit.io internally fetches the file from`https://storagesamples.blob.core.windows.net/rest-of-the-path.jpg`.

{% tabs %}
{% tab title="URL structure" %}
```markup
            URL-endpoint                transformation      image path                                    
┌─────────────────────────────────────┐┌─────────────┐┌───────────────────┐
https://ik.imagekit.io/your_imagekit_id/tr:w-300,h-300/rest-of-the-path.jpg
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
🧙♂**Tips:** You can also use a [custom domain](../../features/using-custom-domain.md) like images.example.com.
{% endhint %}

## Step 3: Integrate and Go live

Now start using ImageKit.io URL endpoint in your application to accelerate image loading.

**Quickly get started with our SDKs:**

{% page-ref page="../../api-reference/api-introduction/sdk.md" %}

**Learn about real-time image resizing:**

{% page-ref page="../../features/image-transformations/" %}

