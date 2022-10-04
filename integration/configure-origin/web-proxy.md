# Web proxy

You can access any image on a publicly-available URL using Web Proxy origin. This allows you to use ImageKit.io's real-time image optimization and transformation features on all existing images.

{% hint style="warning" %}
**Prevent account abuse**\
****If you enable this origin on an [URL-endpoint](../url-endpoints.md), it allows anyone to access any image using your account. To prevent this abuse, we strongly recommend you [restrict the use of unsigned URLs](../../features/security/#restricting-unsigned-urls). Do refer to the information provided after Step 2 of this guide about encoding URLs before signing them.
{% endhint %}

## Step 1: Configure origin

1. Go to the [external storage section](https://imagekit.io/dashboard#external-storage) in your ImageKit.io dashboard, and under the Origins section, click on the "Add origin" button.
2. Choose **Web proxy** from the origin-type dropdown.
3. Give your origin a name. It will appear in the list of origins you have added. For example - **My proxy origin.**
4. Leave the [advanced options](web-server-origin.md#advanced-options-for-web-server-origin) as it is for now.
5. Click on the Submit button.

{% hint style="info" %}
:man_mage:**Whitelist request from ImageKit.io**\
****Make sure that the image public URL is accessible from ImageKit.io. [Learn more](web-server-origin.md#whitelist-request-from-imagekit-io).
{% endhint %}

## Step 2: Access the image through ImageKit.io URL-endpoint

When you add your first origin in the dashboard, the origin is by default made accessible through the [default URL-endpoint](../url-endpoints.md#default-url-endpoint) of your ImageKit.io account. For subsequent origins, you can either create a separate URL-endpoint or edit the existing URL-endpoint (including default) and make this newly added origin accessible by editing the [origin preference list](../url-endpoints.md#image-origin-preference). 

Let's look at a few examples to fetch the images:

* **Image public URL (old URL)**\
  ****[https://www.example.com/rest-of-the-path.jpg](https://www.example.com/rest-of-the-path.jpg)
* **The same master image using ImageKit.io URL-endpoint**\
  ****[https://ik.imagekit.io/your_imagekit_id/https://www.example.com/rest-of-the-path.jpg](https://ik.imagekit.io/your_imagekit_id/https://www.example.com/rest-of-the-path.jpg)
* **Resized 300x300 image**\
  ****[https://ik.imagekit.io/your_imagekit_id/`tr:w-300,h-300`/https://www.example.com/rest-of-the-path.jpg](https://ik.imagekit.io/your_imagekit_id/tr:w-300,h-300/https://www.example.com/rest-of-the-path.jpg)

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

If you get a "Not found" error while accessing the image, check out this [troubleshooting guide](../../limits-and-troubleshooting/404-not-found-error-troubleshooting.md).

{% hint style="info" %}
:man_mage:**Tips:** You can also use a [custom domain](../../testing-and-infrastructure-setup/using-custom-domain-name.md) like images.example.com.
{% endhint %}

{% hint style="info" %}
If you want to create a **signed URL** that uses a Web Proxy origin, you must encode the complete URL of the input image before signing it. For example, instead of using `https://example.com/image.jpg`as input for the signed URL, you should use `https%3A%2F%2Fexample.com%2Fimage.jpg`.
{% endhint %}

## Step 3: Integrate and Go live

Now start using ImageKit.io URL endpoint in your application to accelerate image loading.

**Get started with our quick start guides and SDKs:**

{% content-ref url="../../getting-started/quickstart-guides/" %}
[quickstart-guides](../../getting-started/quickstart-guides/)
{% endcontent-ref %}

{% content-ref url="../../api-reference/api-introduction/sdk.md" %}
[sdk.md](../../api-reference/api-introduction/sdk.md)
{% endcontent-ref %}

**Learn about real-time image resizing:**

{% content-ref url="../../features/image-transformations/" %}
[image-transformations](../../features/image-transformations/)
{% endcontent-ref %}
