# Web proxy

You can access any image on a publicly-available URL using Web Proxy origin. This allows you to use ImageKit.io's real-time image optimization and transformation features on all existing images.

{% hint style="warning" %}
**Prevent account abuse**  
If you enable this origin on an [URL-endpoint](../url-endpoints.md), it allows anyone to access any image using your account. To prevent this abuse, we strongly recommend you [restrict the use of unsigned URLs](../../features/security/#restricting-unsigned-urls).
{% endhint %}

## Step 1: Configure origin

1. Go to the [external storage section](https://imagekit.io/dashboard#external-storage) in your ImageKit.io dashboard, and under the Origins section, click on the "Add origin" button.
2. Choose **Web proxy** from the origin type dropdown.
3. Give your origin a name. It will appear in the list of origins you have added. For example - **My proxy origin.**
4. Leave the [advanced options](web-server-origin.md#advanced-options-for-web-server-origin) as it is for now.
5. Click on the Submit button.

{% hint style="info" %}
🧙♂**Whitelist request from ImageKit.io**  
Make sure that the image public URL is accessible from ImageKit.io. [Learn more](web-server-origin.md#whitelist-request-from-imagekit-io).
{% endhint %}

## Step 2: Access the image through ImageKit.io URL-endpoint

When you add your first origin in the dashboard, the origin is by default made accessible through the [default URL-endpoint](../url-endpoints.md#default-url-endpoint) of your ImageKit.io account. For subsequent origins, you can either create a separate URL-endpoint or edit existing URL-endpoint \(including default\) and make this newly added origin accessible by editing the [origin preference list](../url-endpoints.md#image-origin-preference). 

Let's look at a few examples to fetch the images:

* **Image public URL \(old URL\)** [https://www.example.com/rest-of-the-path.jpg](https://www.example.com/rest-of-the-path.jpg)
* **The same master image using ImageKit.io URL-endpoint** [https://ik.imagekit.io/your\_imagekit\_id/https://www.example.com/rest-of-the-path.jpg](https://ik.imagekit.io/your_imagekit_id/https://www.example.com/rest-of-the-path.jpg)
* **Resized 300x300 image** [https://ik.imagekit.io/your\_imagekit\_id/`tr:w-300,h-300`/https://www.example.com/rest-of-the-path.jpg](https://ik.imagekit.io/your_imagekit_id/tr:w-300,h-300/https://www.example.com/rest-of-the-path.jpg)

So when you request `https://ik.imagekit.io/your_imagekit_id/https://www.example.com/rest-of-the-path.jpg`, ImageKit.io internally fetches the file from `https://www.example.com/rest-of-the-path.jpg`.

{% tabs %}
{% tab title="URL structure" %}
```markup
            URL-endpoint                transformation              image public URL                                    
┌─────────────────────────────────────┐┌─────────────┐┌───────────────────────────────────────────┐
https://ik.imagekit.io/your_imagekit_id/tr:w-300,h-300/https://www.example.com/rest-of-the-path.jpg

```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
🧙♂**Tips:** You can also use a [custom domain](../../features/using-custom-domain.md) like images.example.com.
{% endhint %}

## Step 3: Integrate and Go live

Now start using ImageKit.io URL endpoint in your application to accelerate image loading.

**Get started with our SDKs:**

{% page-ref page="../../api-reference/api-introduction/sdk.md" %}

**Learn about real-time image resizing:**

{% page-ref page="../../features/image-transformations/" %}
