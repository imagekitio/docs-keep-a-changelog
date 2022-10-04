# Azure Blob storage

You can configure ImageKit.io to fetch images from your Azure blob storage. This allows you to start leveraging ImageKit.io real-time image resizing, optimization, and fast CDN delivery for thousands or millions of existing images within minutes.

**Note:** We do not start copying images from your storage as soon as you add it. Instead, we will fetch the particular image when you request it through ImageKit.io URL-endpoint. [Learn more](../how-it-works.md) to understand how this works. The images accessed from this origin will not appear in your [Media library](../../media-library/overview/).

The following tutorial will help you create a **Shared Access Signature (SAS)** token that will allow ImageKit to access the images in your Azure storage container. This allows you to restrict ImageKit to only reading objects from your container. i.e. ImageKit will not be able to create/delete/update objects in your container.

{% hint style="info" %}
In case you find the following instructions invalid/outdated, you can refer to Azure's official, albeit more verbose instructions on [this page](https://docs.microsoft.com/en-us/azure/storage/common/storage-sas-overview).
{% endhint %}

{% hint style="info" %}
If you have granted anonymous public access to your Azure container, you can choose to configure your container as a web server. Provide your container URL as the web server URL in the web server configuration steps.[ See how to configure a web server.](https://docs.imagekit.io/integration/configure-origin/web-server-origin)

However, note that it is advisable to configure your origin using secure integration as explained below.
{% endhint %}

## Step 1: Generate a Shared Access Signature (SAS) Token

1. Sign in to your Azure Account through the [Azure portal](https://portal.azure.com).
2. Select **Storage Accounts**.
3. Select the corresponding storage account.
4. Search for the Shared Access Signature setting in the settings pane on the left.
5. Now, select the options as shown in the following image. This ensures that ImageKit is allowed to only read images from your Azure container.
6. Enter an **End** expiry time that is practically infinite. Preferably, enter a time 10 years from the start time (which should be right now)
7. Click on 'Generate SAS and connection string'
8. Note down the SAS token generated. It should be of the format `?sv=2019-12-12&ss=bfqt.........`

![Options for creating a SAS token](<../../.gitbook/assets/image (5).png>)

{% hint style="info" %}
This method generates an account-level Shared Access Signature. To generate a service SAS or a user-delegated SAS, visit the [official Azure documentation pages here](https://docs.microsoft.com/en-us/azure/storage/common/storage-sas-overview).
{% endhint %}

## Step 2: Configure your Azure storage container with your ImageKit account

We have now created a SAS token for ImageKit and granted it the Read permission for your container, which means that ImageKit can do nothing more than just read objects in your container. At this point, you should have the following values with you:

* **Azure storage account name: **The name of your storage account.
* **Azure storage container name: **Name of the container that you want to integrate.
* **SAS Token: **As generated in step 1. Do note that the SAS token should begin with a `?` when adding it with the origin in ImageKit. For example - `?sv=2019-12-12&ss=bfqt.........`

Now, go to the [External Storage](https://imagekit.io/dashboard#external-storage) section in the dashboard, click on the **Add New Origin **button, select Azure Storage in the **Origin Type** field, and enter the corresponding values, and click submit.

## Step 3: Access the image through ImageKit.io URL-endpoint

When you add your first origin in the dashboard, the origin is by default made accessible through the [default URL-endpoint](../url-endpoints.md#default-url-endpoint) of your ImageKit.io account. For subsequent origins, you can either create a separate URL-endpoint or edit the existing URL-endpoint (including default) and make this newly added origin accessible by editing the [origin preference list](../url-endpoints.md#image-origin-preference). 

Let's look at a few examples to fetch the images:

* **Original image through Azure blog storage (old URL)**\
  [https://storagesamples.blob.core.windows.net/rest-of-the-path.jpg](https://storagesamples.blob.core.windows.net/rest-of-the-path.jpg)
* **The same master image using ImageKit.io URL-endpoint**\
  [https://ik.imagekit.io/your_imagekit_id/rest-of-the-path.jpg](https://ik.imagekit.io/your_imagekit_id/rest-of-the-path.jpg)
* **Resized 300x300 image**\
  [https://ik.imagekit.io/your_imagekit_id/`tr:w-300,h-300`/rest-of-the-path.jpg](https://ik.imagekit.io/your_imagekit_id/tr:w-300,h-300/rest-of-the-path.jpg)

So when you request `https://ik.imagekit.io/your_imagekit_id/rest-of-the-path.jpg`, \
ImageKit.io uses the SAS token provided by you to fetch the original image from the path `rest-of-the-path.jpg` in the Azure container using the official Azure SDK.

{% tabs %}
{% tab title="URL structure" %}
```markup
            URL-endpoint                transformation      image path                                    
┌─────────────────────────────────────┐┌─────────────┐┌───────────────────┐
https://ik.imagekit.io/your_imagekit_id/tr:w-300,h-300/rest-of-the-path.jpg
```
{% endtab %}
{% endtabs %}

If you get a "Not found" error while accessing the image, check out this [troubleshooting guide](../../limits-and-troubleshooting/404-not-found-error-troubleshooting.md).

{% hint style="info" %}
:man_mage:**Tips:** You can also use a [custom domain](../../testing-and-infrastructure-setup/using-custom-domain-name.md) like images.example.com.
{% endhint %}

## Step 4: Integrate and Go live

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
